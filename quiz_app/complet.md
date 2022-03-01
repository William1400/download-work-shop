**App.tsx**
```tsx
import React, { useState } from 'react';
import { fetchQuizQuestions } from './API';

// Components
import QuestionCard from './components/QuestionCard';

// Types 
import { QuestionState, Difficulty } from './API';

// Styles

export type AnswerObject = {

    question: string;
    answer: string;
    correct: boolean;
    correctAnswer: string;
}

const TOTAL_QUESTIONS = 15;

function App() {

    const [loading, setLoading] = useState(false);
    const [questions, setQuestions] = useState<QuestionState[]>([]);
    const [number, setNumber] = useState(0);
    const [userAnswers, setUserAnswers] = useState<AnswerObject[]>([]);
    const [score, setScore] = useState(0);
    const [gameOver, setGameOver] = useState(true);

    const startGame = async () => {

        setLoading(true);
        setGameOver(false);

        const newQuestions = await fetchQuizQuestions(TOTAL_QUESTIONS, Difficulty.EASY);

        setQuestions(newQuestions);
        setScore(0);
        setUserAnswers([]);
        setNumber(0);
        setLoading(false);
    };

    const checkAnswer = (e: React.MouseEvent<HTMLButtonElement>) => {

        if (!gameOver) {

            //Users Answer
            const answer = e.currentTarget.value;
            // Check answer against correct answer
            const correct = questions[number].correct_answer === answer;
            // Add score if answer is correct
            if (correct) setScore(prev => prev + 1);
            // Save answer in the array in the array for user userAnswers
            const answerObject = {

                question: questions[number].question,
                answer,
                correct,
                correctAnswer: questions[number].correct_answer,
            };

            setUserAnswers((prev) => [...prev, answerObject]);
        }
    };

    const nextQuestion = () => {

        // Move on the next question if not the last question
        const nextQuestion = number + 1;

        if (nextQuestion === TOTAL_QUESTIONS) {

            setGameOver(true);
        } else {

            setNumber(nextQuestion);
        }
    };

    return (

        <>
            <div className="App">
                <h1>React Quiz</h1>
                {gameOver || userAnswers.length === TOTAL_QUESTIONS ? (

                    <button className="start" onClick={startGame}>Start</button>
                ) : null}
                {!gameOver ? <p className="score">Score: {score}</p> : null}
                {loading && <p>Loading Question...</p>}
                {!loading && !gameOver && (

                    <QuestionCard
                        questionNumber={number + 1}
                        totalQuestions={TOTAL_QUESTIONS}
                        question={questions[number].question}
                        answers={questions[number].answers}
                        userAnswer={userAnswers ? userAnswers[number] : undefined}
                        callback={checkAnswer}
                    />
                )}
                {!gameOver && !loading && userAnswers.length === number + 1 && number !== TOTAL_QUESTIONS - 1 ? (

                    <button className="next" onClick={nextQuestion}>Next Question</button>
                ) : null}
            </div>
        </>
    );
}

export default App;

```

**API.ts**

```tsx
import { shuffleArray } from './utils';

export type Question = {

    category: string;
    correct_answer: string;
    difficulty: string;
    incorrect_answers: string[];
    question: string;
    type: string;
}

export type QuestionState = Question & { answers: string[]};

export enum Difficulty {

    EASY = "easy",
    MEDIUM = "medium",
    HARD = "hard",
}

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

**components/QuestionCard**

```tsx
import React from 'react';
import { AnswerObject } from '../App'

type Props = {

    question: string;
    answers: string[];
    callback: (e: React.MouseEvent<HTMLButtonElement>) => void;
    userAnswer: AnswerObject | undefined;
    questionNumber: number;
    totalQuestions: number;
}
const QuestionCard: React.FC<Props> = ({
    
    question,
    answers,
    callback,
    userAnswer,
    questionNumber,
    totalQuestions
}) => (

    <div>
        <p className="number">
            Question: {questionNumber} / {totalQuestions}
        </p>
        <p dangerouslySetInnerHTML={{ __html: question }}/>
        <div>
            {answers.map(answer => (

                <div key={answer}>
                    <button disabled={userAnswer ? true : false} value={answer} onClick={callback}>
                        <span dangerouslySetInnerHTML= {{ __html: answer }}></span>
                    </button>
                </div>
            ))}
        </div>
    </div>
);

export default QuestionCard;
```

**utils.ts**
```tsx
export const shuffleArray = (array: any[]) => 

    [...array].sort(() => Math.random() -0.5);

```

[aller au tutoriel du quiz multiplayer](https://github.com/WilliamLoey/WoorkShop-tutorial-)