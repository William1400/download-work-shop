# QUIZZ PROJECT

Programmation d'un quizz React TypeScript à l'aide d'une API externe.

### Installation

- clone ce repo
- cd app
- ``npm install``


<br>
<br>

### Première étape

Voici à quoi doit ressembler votre App.tsx au départ

**App.tsx**
```tsx
import React, { useState } from 'react';

// Components

// Types 

// Styles

function App() {

    return (

        <>
            <div className="App">
                Welcome
            </div>
        </>
    );
}

export default App;
```
Dans le même repertoire, nous allons ensuite créer un dossier **components**.
Nous allons y intégrer notre premier component: **QuestionCard.tsx**

**QuestionCard.tsx**

```tsx
import React from 'react';

// types

// styles

// type

const QuestionCard = () => (

    <div>
    QuestionCard
    </div>
);

export default QuestionCard;
```

<br>
<br>

## Seconde étape

Nous pouvons maintenant importer notre component QuestionCard.tsx dans l'App.tsx


```tsx
import QuestionCard from './components/QuestionCard';
```
Nous aurons besoin de 3 fonctions que nous allons déjà initiés:
- startGame() : qui démarre le quizz
- checkAnswer(``e: React.MouseEvent<HTMLButtonElement>``) : qui vérifie si la réponse selectionnée est la bonne.
- NextQuestion() : qui passe à la question suivante.
  

Dans **App.tsx**, à l'interieur de la fonction App
```tsx
const startGame = async () => {};

const checkAnswer = (e: React.MouseEvent<HTMLButtonElement>) => {};

const nextQuestion = () => {};

```


Nous allons maintenant préparer les props pour notre component:
**QuestionCard**

Comme vu précédement, il faut définir le **type** des props:

```tsx
type Props = {

    question: string;
    answers: string[];
    callback: (/* e: React.MouseEvent<HTMLButtonElement> */) => void;
    userAnswer: /* AnswerObject | */ undefined;
    questionNumber: number;
    totalQuestions: number;
};
```
On passe ensuite en paramètre les props dans la fonction **QuestionCard**
```tsx
const QuestionCard: React.FC<Props> = ({
    
    question,
    answers,
    callback,
    userAnswer,
    questionNumber,
    totalQuestions
}) => (

    <div></div>
)
```
<br>
<br>

## Troisième étape
Nous allons maintenant implémenter les Hooks.
Il faut décommenter useState pour implémenter les hooks.

**App.tsx**

```tsx
const [loading, setLoading] = useState(false);
const [questions, setQuestions] = useState/* <QuestionState[]> */([]);
const [number, setNumber] = useState(0);
const [userAnswers, setUserAnswers] = useState/* <AnswerObject[]> */([]);
const [score, setScore] = useState(0);
const [gameOver, setGameOver] = useState(true);

```

Nous avons assignés 2 types que nous n'avons pas défini encore **QuestionState** et **AnswerObject**.
Crée un fichier API.ts (src/**API.ts**) et dans ce fichier nous allons fetch l'API;

Définissons **QuestionState**:
**API.ts**
```tsx
//il faut détailler l'objet question
export type Question = {

    category: string;
    correct_answer: string;
    difficulty: string;
    incorrect_answers: string[];
    question: string;
    type: string;
}


// export question via Question State
export type QuestionState = Question & { answers: string[]};
```
Ensuite préparons un objet qui nous servira pour l'API: **Difficulty**;

```tsx
// export objet question difficulter
export enum Difficulty {

    EASY = "easy",
    MEDIUM = "medium",
    HARD = "hard",
}
```
**enum** permet de définir un ensemble de constantes nommées. L'utilisation des enums peut faciliter la documentation, ou la création d'un ensemble de cas distincts. TypeScript fournit à la fois des enums numériques et des enums basés sur des chaînes de caractères.

<br>

Créer le fichier et mettre **utils.ts** dans le dossier *src*

ici, nous créons un *array* pour passer la fonction aléatoire avec random et générer aléatoirement nos questions 

```tsx
// quick fix random const
export const shuffleArray = (array: any[]) =>

    [...array].sort(() => Math.random() - 0.5);



```
**API.ts**
```tsx
import {shuffleArray} from "./utils.ts"
// aller regarder la promise localhost
```

Dans API.ts, nous allons fetch l'API externe:

```tsx
// fetch asynchrone sur l'API + les deux objets que l'on a besoin 
export const fetchQuizQuestions = async (amount: number, difficulty: Difficulty) => {

    const endpoint = `https://opentdb.com/api.php?amount=${amount}&difficulty=${difficulty}&type=multiple`;
    const data = await (await fetch(endpoint)).json();
    
    return data.results.map((question: Question) => ({

        ...question,
        answers: shuffleArray([
            
            ...question.incorrect_answers, question.correct_answer,
        ]),
    }));
    
};
```
<br>
Nous allons instancier *TOTAL_QUESTIONS* 
**App.tsx**
```tsx
// à placer au dessus de la fonction App
const TOTAL_QUESTIONS = 15;
```
Modifier le bloc return dans 

**App.tsx**

```tsx


    return (

            <div className="App" >
            
                <h1>React Quiz</h1>
            
                <QuestionCard
                       
                       questionNumber={number + 1}
                       totalQuestions={TOTAL_QUESTIONS}
                       question={questions[number].question}
                       answers={questions[number].answers}
                       userAnswer={userAnswers ? userAnswers[number] : undefined}
                       callback={checkAnswer}
                />
                
                <button className="next" onClick={nextQuestion}>Next Question</button>
               
            
            </div>
    );

```


Maintenant, allez à la suite pour integrer les Types [ici](./aplication.md)
