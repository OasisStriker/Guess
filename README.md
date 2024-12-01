'use client'

import { useState, useEffect } from 'react'
import { Button } from "@/components/ui/button"
import { Input } from "@/components/ui/input"
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"

export default function GuessTheNumber() {
  const [targetNumber, setTargetNumber] = useState(0)
  const [guess, setGuess] = useState('')
  const [feedback, setFeedback] = useState('')
  const [attempts, setAttempts] = useState(0)
  const [gameWon, setGameWon] = useState(false)

  useEffect(() => {
    resetGame()
  }, [])

  const resetGame = () => {
    setTargetNumber(Math.floor(Math.random() * 100) + 1)
    setGuess('')
    setFeedback('Make a guess!')
    setAttempts(0)
    setGameWon(false)
  }

  const handleGuess = (e: React.FormEvent) => {
    e.preventDefault()
    const numberGuess = parseInt(guess)

    if (isNaN(numberGuess)) {
      setFeedback('Please enter a valid number')
      return
    }

    setAttempts(attempts + 1)

    if (numberGuess === targetNumber) {
      setFeedback(`Congratulations! You guessed the number in ${attempts + 1} attempts!`)
      setGameWon(true)
    } else if (numberGuess < targetNumber) {
      setFeedback('Too low! Try a higher number.')
    } else {
      setFeedback('Too high! Try a lower number.')
    }
  }

  return (
    <Card className="w-full max-w-md mx-auto">
      <CardHeader>
        <CardTitle className="text-2xl font-bold text-center">Guess the Number</CardTitle>
      </CardHeader>
      <CardContent>
        <p className="text-center mb-4">I'm thinking of a number between 1 and 100.</p>
        <form onSubmit={handleGuess} className="space-y-4">
          <Input
            type="number"
            value={guess}
            onChange={(e) => setGuess(e.target.value)}
            placeholder="Enter your guess"
            min="1"
            max="100"
            disabled={gameWon}
            className="text-center"
          />
          <div className="flex justify-center space-x-2">
            <Button type="submit" disabled={gameWon}>
              Guess
            </Button>
            <Button type="button" onClick={resetGame} variant="outline">
              New Game
            </Button>
          </div>
        </form>
        <p className="mt-4 text-center font-semibold">{feedback}</p>
        <p className="mt-2 text-center">Attempts: {attempts}</p>
      </CardContent>
    </Card>
  )
}

