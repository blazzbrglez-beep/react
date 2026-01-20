1. Interface
export interface Aktivnost {
  id: number;
  vrsta: string;
  razdalja: number;
  trajanje: number;
  datum: string;
  opis: string;
}

2. App.tsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import { useState } from 'react';
import Home from './components/Home';
import AddActivity from './components/AddActivity';
import Details from './components/Details';
import { Aktivnost } from './types';

function App() {
  const [aktivnosti, setAktivnosti] = useState<Aktivnost[]>([]);

  const dodajNovo = (a: Aktivnost) => {
    setAktivnosti([...aktivnosti, a]);
  };

  return (
    <BrowserRouter>
      <div className="container">
        <Routes>
          <Route path="/" element={<Home seznam={aktivnosti} />} />
          <Route path="/dodaj" element={<AddActivity onAdd={dodajNovo} />} />
          {/* Pot s parametrom :id za podrobnosti */}
          <Route path="/details/:id" element={<Details seznam={aktivnosti} />} />
        </Routes>
      </div>
    </BrowserRouter>
  );
}

export default App;

3.Home.tsx

import { Link } from 'react-router-dom';
import { Aktivnost } from '../types';

function Home({ seznam }: { seznam: Aktivnost[] }) {
  return (
    <div>
      <h1>Moje Aktivnosti</h1>
      <Link to="/dodaj"> Dodaj novo aktivnost </Link>
      
      <ul>
        {seznam.map(a => (
          <li key={a.id}>
            {/* Klik na aktivnost pelje na podrobnosti */}
            <Link to={`/details/${a.id}`}>
              {a.vrsta} - {a.datum}
            </Link>
          </li>
        ))}
      </ul>
    </div>
  );
}
export default Home;

4. AddActivity.tsx

import { useState } from 'react';
import { useNavigate } from 'react-router-dom';

function AddActivity({ onAdd }: { onAdd: (a: any) => void }) {
  const navigate = useNavigate();
  const [vrsta, setVrsta] = useState('Tek');

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    const nova = { id: Date.now(), vrsta, datum: new Date().toLocaleDateString(), ... };
    onAdd(nova);
    navigate('/'); // <--- TOLE te vrne na domov
  };

  return (
    <form onSubmit={handleSubmit}>
      <select value={vrsta} onChange={e => setVrsta(e.target.value)}>
        <option value="Tek">Tek</option>
        <option value="Kolo">Kolo</option>
        <option value="Plavanje">Plavanje</option>
      </select>
      <button type="submit">Shrani</button>
    </form>
  );
}

5. Details.tsx

import { useParams, Link } from 'react-router-dom';
import { Aktivnost } from '../types';

function Details({ seznam }: { seznam: Aktivnost[] }) {
  const { id } = useParams(); // Prebere ID iz URL-ja (npr. /details/123)
  const aktivnost = seznam.find(a => a.id === Number(id));

  if (!aktivnost) return <p>Ni najdeno! <Link to="/">Nazaj</Link></p>;

  return (
    <div>
      <h2>Podrobnosti: {aktivnost.vrsta}</h2>
      <p>Razdalja: {aktivnost.razdalja} km</p>
      <p>Opis: {aktivnost.opis}</p>
      <Link to="/">Nazaj na seznam</Link>
    </div>
  );
}