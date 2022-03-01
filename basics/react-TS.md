# GET STARTED REACT TYPESCRIPT

## INSTALLATION

Pour démarrer un nouveau projet react app avec TypeScript, vous pouvez exécuter :

```npx create-react-app my-app --template typescript```

Pour ajouter TypeScript à un projet React App existant, commencez par l'installer :

```npm install --save typescript @types/node @types/react @types/react-dom @types/jest```

<br>
<br>

## FUNCTION COMPONENT & BASIC PROPS

Nous allons créer notre première fonction component.
D'abords nous allons définir les props que nous allons
passer en arguments plus tard.

```tsx
interface AppProps{
  headerText: string;
  message: string;

}
```
Nous pouvons définir notre fonction **App**:

```tsx

function App({ headerText, message }: AppProps) {
  return (
    <div className="App">
      <h1>{headerText}</h1>
      {message && <p>{message}</p>}
    </div>
  );
}
```

**Attention !** Il faut définir le type des props avant de les utiliser.
Ici ``{ headerText, message}`` ont le type définit par **AppProps**.
Nous pouvons ajouter des *props conditionnels* en ajoutant ``?`` devant le props ``message?: string``

ici:
```tsx
interface AppProps {
    headerText: string;
    message?: string;
}
```
De cette manière, si il n'y a pas de prop **message** aucune erreur ne sera affichée.
<br>
<br>

## HOOKS
**useState** nous permettra d'utiliser des fonctions et des variables sur notre application

**Type Inference**

Lorsque nous déclarons notre variable, un type est infèré automatiquement. Si TS ne reconnait pas la variable, **any** sera inféré.
  ```
  const [user, setUser] = useState('name');
  ```
Notre variable est donc un *string*

Cette méthode peut tout de fois améner à des erreurs, il vaut donc mieux déclarer les types de vos props.

Nous le faisons ici avec une *Interface*.

**Type Annotation**

  ```tsx
  interface User{
      name: string,
      age: number,
  }
  ```

  notre useState devient donc:
  ```tsx
  const [user, setUser] = useState<User | null>(null)
  ```

  
```tsx
interface User{
    name: string;
    age: number;
}

function App () {
    const [user, setUser] = useState<User | null>(null);

    const fetchUser = () => setUser({
        name: 'Michel',
        age: 23,
})


  return (
    <div className="App">
        <h1>Use State</h1>
        <button onClick={fetchUser}> Fetch User On Click</button>
        {user && <p>{`Bonjour ${user.name}`}</p>}
    </div>
  );
}

```
<br>

[Aller à la création de quiz !!](./quiz_app/complet.md)