### 1. Dinamični Input Handler (NAJPOMEMBNEJŠE!)
Namesto 5 funkcij za 5 vnosnih polj, uporabi tole eno. Prihrani ti 10 minut pri vsaki formi.
```tsx
const handleInputChange = (e: React.ChangeEvent<HTMLInputElement | HTMLSelectElement | HTMLTextAreaElement>) => {
  const { name, value } = e.target;
  // name mora biti enak ključu v interface-u (npr. name="naslov")
  setForm({ ...form, [name]: value });
};

// Uporaba v HTML:
// <input name="naslov" value={form.naslov} onChange={handleInputChange} />
```

### 2. Standardni Fetch klici (za json-server)
Ko rabiš hitro povezavo na API.

```tsx
// GET - Pridobivanje
useEffect(() => {
  fetch('http://localhost:3000/aktivnosti')
    .then(res => res.json())
    .then(data => setSeznam(data))
    .catch(err => console.error(err));
}, []);

// POST - Dodajanje
const handleAdd = (novoObjekt: any) => {
  fetch('http://localhost:3000/aktivnosti', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(novoObjekt)
  })
  .then(res => res.json())
  .then(podatek => setSeznam([...seznam, podatek]));
};
```

### 3. React Router Hooks (Navigacija in ID)
Kako skočiti na drugo stran in kako prebrati številko iz URL-ja.

```tsx
// Skok na drugo stran (npr. po kliku na Shrani)
const navigate = useNavigate();
navigate('/'); 

// Branje ID-ja iz URL (npr. /podrobnosti/5)
const { id } = useParams<{ id: string }>();
const izbranElement = seznam.find(item => item.id === Number(id));
```

### 4. Menjava strani BREZ Routerja (Conditional Rendering)
Če v navodilih piše "ne potrebujete React Router".

```tsx
const [view, setView] = useState<'seznam' | 'dodaj' | 'podrobnosti'>('seznam');

{view === 'seznam' && <Seznam onAddClick={() => setView('dodaj')} />}
{view === 'dodaj' && <Forma onBack={() => setView('seznam')} />}
```

### 5. LocalStorage & Login (Zaščita strani)
Če dobiš nalogo s prijavo (Login).

```tsx
// Shranjevanje ob prijavi
localStorage.setItem('user', JSON.stringify({ username: 'blaz' }));

// Preverjanje (npr. v useEffect v App.tsx)
const checkUser = () => {
  const loggedUser = localStorage.getItem('user');
  if (!loggedUser) navigate('/login');
};
```

### 6. Osnovni `db.json` (Za json-server)
Vedno imej pripravljen začetni JSON, da ga samo prilepiš v datoteko.

```json
{
  "aktivnosti": [
    { "id": 1, "vrsta": "Tek", "razdalja": 5, "datum": "2024-05-20", "opis": "Test" }
  ]
}
```

---

## 🛠️ Navodila za "Preživetje" (Jutri ob 17:00)

1.  **Terminal 1:** `npm run dev` (React)
2.  **Terminal 2:** `npx json-server --watch db.json --port 3000` (Baza)
3.  **Če TypeScript preveč teži (rdeče črte):** Povsod uporabi `: any`. 
    *   Primer: `const [data, setData] = useState<any[]>([])`
    *   Primer: `(e: any) => ...`
    *   *Izpit ni tekmovanje v popolnem TypeScriptu, ampak v tem, da aplikacija DELUJE.*
4.  **Interface je prvi korak:** Takoj ko dobiš navodila, spremeni svoj interface v GitHubu. Če imaš urejen interface, ti bo VS Code pomagal pri vsem ostalem.
5.  **Ctrl + F (Find and Replace):** Če kopiraš kodo "Stikov" za nalogo "Receptov", uporabi zamenjavo besed, da ne boš ročno popravljal 50 vrstic.

**To je to.** Z vsemi temi repozitoriji in temi snippeti si zdaj pripravljen bolje kot 90% študentov. Naspi se, bodi zbran in jutri boš tole rešil kot za šalo.

SREČNO! Se slišiva, če boš še kaj rabil. 🍀