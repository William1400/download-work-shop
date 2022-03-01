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

