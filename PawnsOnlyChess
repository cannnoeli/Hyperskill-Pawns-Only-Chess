package chess

data class Pawn(val color: String, var file: Int, var rank: Int)
data class Player(val name: String, val color: String)

fun setPawns(): MutableList<Pawn> {
    val pawns = mutableListOf<Pawn>()
    for (i in 1..8) {
        pawns.add(Pawn("W", i, 2))
        pawns.add(Pawn("B", i, 7))
    }
    return pawns
}

fun printChessboard(pawns: MutableList<Pawn>) {
    val chessBorder = "  +---+---+---+---+---+---+---+---+"
    val emptySpot = "   |"

    for (rank in 8 downTo 1) {
        println(chessBorder)

        print("$rank |")

        for (file in 1..8) {
            if (Pawn("W", file, rank) in pawns) {
                print(" W |")
            } else if (Pawn("B", file, rank) in pawns) {
                print(" B |")
            } else {
                print(emptySpot)
            }
        }
        print("\n")
    }

    println(chessBorder)
    println("    a   b   c   d   e   f   g   h\n")
}

fun playChess(wPlayer: Player, bPlayer: Player, pawns: MutableList<Pawn>) {
    val legalMove = "[a-h][1-8][a-h][1-8]".toRegex()
    var enPassant: Pawn? = null

    fun makeAMove(player: Player): Int {
        getMove@while (true) {
            var validMoves = 0
            for (pawn in pawns) {
                if (player.color == pawn.color) {
                    when (pawn.color) {
                        "W" -> {
                            when (pawn.rank) {
                                2 -> {
                                    val move1 = Pawn(pawn.color, pawn.file, pawn.rank + 1)
                                    val move2 = Pawn(pawn.color, pawn.file, pawn.rank + 2)

                                    if (Pawn("B", move1.file, move1.rank) !in pawns && move1 !in pawns) {
                                        validMoves++
                                        break
                                    }

                                    if (Pawn("B", move2.file, move2.rank) !in pawns && move2 !in pawns) {
                                        validMoves++
                                        break
                                    }
                                }

                                else -> {
                                    val move1 = Pawn(pawn.color, pawn.file, pawn.rank + 1)

                                    if (Pawn("B", move1.file, move1.rank) !in pawns && move1 !in pawns) {
                                        validMoves++
                                        break
                                    }
                                }
                            }
                            when (pawn.file) {
                                1 -> {
                                    val enemyPawn1 = Pawn("B", pawn.file + 1, pawn.rank + 1)

                                    if (enemyPawn1 in pawns || enemyPawn1 == enPassant) {
                                        validMoves++
                                        break
                                    }
                                }
                                in 2..7 -> {
                                    val enemyPawn1 = Pawn("B", pawn.file + 1, pawn.rank + 1)
                                    val enemyPawn2 = Pawn("B", pawn.file - 1, pawn.rank + 1)

                                    if (enemyPawn1 in pawns || enemyPawn1 == enPassant) {
                                        validMoves++
                                        break
                                    }

                                    if (enemyPawn2 in pawns || enemyPawn2 == enPassant) {
                                        validMoves++
                                        break
                                    }
                                }
                                8 -> {
                                    val enemyPawn1 = Pawn("B", pawn.file - 1, pawn.rank + 1)

                                    if (enemyPawn1 in pawns || enemyPawn1 == enPassant) {
                                        validMoves++
                                        break
                                    }
                                }
                            }
                        }

                        "B" -> {
                            when (pawn.rank) {
                                7 -> {
                                    val move1 = Pawn(pawn.color, pawn.file, pawn.rank - 1)
                                    val move2 = Pawn(pawn.color, pawn.file, pawn.rank - 2)

                                    if (Pawn("W", move1.file, move1.rank) !in pawns && move1 !in pawns) {
                                        validMoves++
                                        break
                                    }
                                    if (Pawn("W", move2.file, move2.rank) !in pawns && move2 !in pawns) {
                                        validMoves++
                                        break
                                    }
                                }

                                else -> {
                                    val move1 = Pawn(pawn.color, pawn.file, pawn.rank - 1)

                                    if (Pawn("W", move1.file, move1.rank) !in pawns && move1 !in pawns){
                                        validMoves++
                                        break
                                    }
                                }
                            }
                            when (pawn.file) {
                                1 -> {
                                    val enemyPawn1 = Pawn("W", pawn.file + 1, pawn.rank - 1)

                                    if (enemyPawn1 in pawns || enemyPawn1 == enPassant) {
                                        validMoves++
                                        break
                                    }
                                }
                                in 2..7 -> {
                                    val enemyPawn1 = Pawn("W", pawn.file + 1, pawn.rank - 1)
                                    val enemyPawn2 = Pawn("W", pawn.file - 1, pawn.rank - 1)

                                    if (enemyPawn1 in pawns || enemyPawn1 == enPassant) {
                                        validMoves++
                                        break
                                    }

                                    if (enemyPawn2 in pawns || enemyPawn2 == enPassant) {
                                        validMoves++
                                        break
                                    }
                                }
                                8 -> {
                                    val enemyPawn1 = Pawn("W", pawn.file - 1, pawn.rank - 1)

                                    if (enemyPawn1 in pawns || enemyPawn1 == enPassant) {
                                        validMoves++
                                        break
                                    }
                                }
                            }
                        }
                    }
                }
            }

            if (validMoves == 0) {
                println("Stalemate!")
                return 1
            }

            println("${player.name}'s turn:")
            val move = readln()

            when {
                move == "exit" -> return 1
                !legalMove.matches(move) -> {
                    println("Invalid Input")
                    continue@getMove
                }
            }
            
            val moveFrom = Pawn(player.color, move[0].code - 96, move[1].toString().toInt())
            
            if (moveFrom !in pawns) {
                println("No ${if (player.color == "W") "white" else "black"} pawn at ${(moveFrom.file + 96).toChar()}${moveFrom.rank}")
                continue@getMove
            }

            val validForwardMoves = mutableListOf<Pawn>()
            val validCaptureMoves = mutableListOf<Pawn?>()

            when (moveFrom.color) {
                "W" -> {
                    when (moveFrom.rank) {
                        2 -> {
                            val move1 = Pawn(moveFrom.color, moveFrom.file, moveFrom.rank + 1)
                            val move2 = Pawn(moveFrom.color, moveFrom.file, moveFrom.rank + 2)

                            if (Pawn("B", move1.file, move1.rank) !in pawns && move1 !in pawns) {
                                validForwardMoves.add(move1)
                            }

                            if (Pawn("B", move2.file, move2.rank) !in pawns && move2 !in pawns) {
                                validForwardMoves.add(move2)
                            }
                        }

                        else -> {
                            val move1 = Pawn(moveFrom.color, moveFrom.file, moveFrom.rank + 1)

                            if (Pawn("B", move1.file, move1.rank) !in pawns && move1 !in pawns) {
                                validForwardMoves.add(move1)
                            }
                        }
                    }
                    when (moveFrom.file) {
                        1 -> {
                            val enemyPawn1 = Pawn("B", moveFrom.file + 1, moveFrom.rank + 1)

                            if (enemyPawn1 in pawns || enemyPawn1 == enPassant) {
                                validCaptureMoves.add(enemyPawn1)
                            }
                        }
                        in 2..7 -> {
                            val enemyPawn1 = Pawn("B", moveFrom.file + 1, moveFrom.rank + 1)
                            val enemyPawn2 = Pawn("B", moveFrom.file - 1, moveFrom.rank + 1)

                            if (enemyPawn1 in pawns || enemyPawn1 == enPassant) {
                                validCaptureMoves.add(enemyPawn1)
                            }

                            if (enemyPawn2 in pawns || enemyPawn2 == enPassant) {
                                validCaptureMoves.add(enemyPawn2)
                            }
                        }
                        8 -> {
                            val enemyPawn1 = Pawn("B", moveFrom.file - 1, moveFrom.rank + 1)

                            if (enemyPawn1 in pawns || enemyPawn1 == enPassant) {
                                validCaptureMoves.add(enemyPawn1)
                            }
                        }
                    }
                }

                "B" -> {
                    when (moveFrom.rank) {
                        7 -> {
                            val move1 = Pawn(moveFrom.color, moveFrom.file, moveFrom.rank - 1)
                            val move2 = Pawn(moveFrom.color, moveFrom.file, moveFrom.rank - 2)

                            if (Pawn("W", move1.file, move1.rank) !in pawns && move1 !in pawns) {
                                validForwardMoves.add(move1)
                            }
                            if (Pawn("W", move2.file, move2.rank) !in pawns && move2 !in pawns) {
                                validForwardMoves.add(move2)
                            }
                        }

                        else -> {
                            val move1 = Pawn(moveFrom.color, moveFrom.file, moveFrom.rank - 1)

                            if (Pawn("W", move1.file, move1.rank) !in pawns && move1 !in pawns){
                                validForwardMoves.add(move1)
                            }
                        }
                    }
                    when (moveFrom.file) {
                        1 -> {
                            val enemyPawn1 = Pawn("W", moveFrom.file + 1, moveFrom.rank - 1)

                            if (enemyPawn1 in pawns || enemyPawn1 == enPassant) {
                                validCaptureMoves.add(enemyPawn1)
                            }
                        }
                        in 2..7 -> {
                            val enemyPawn1 = Pawn("W", moveFrom.file + 1, moveFrom.rank - 1)
                            val enemyPawn2 = Pawn("W", moveFrom.file - 1, moveFrom.rank - 1)

                            if (enemyPawn1 in pawns || enemyPawn1 == enPassant) {
                                validCaptureMoves.add(enemyPawn1)
                            }

                            if (enemyPawn2 in pawns || enemyPawn2 == enPassant) {
                                validCaptureMoves.add(enemyPawn2)
                            }
                        }
                        8 -> {
                            val enemyPawn1 = Pawn("W", moveFrom.file - 1, moveFrom.rank - 1)

                            if (enemyPawn1 in pawns || enemyPawn1 == enPassant) {
                                validCaptureMoves.add(enemyPawn1)
                            }
                        }
                    }
                }
            }

            val forwardMove = Pawn(moveFrom.color, move[2].code - 96, move[3].toString().toInt())
            val captureMove = Pawn(if (moveFrom.color == "W") "B" else "W", move[2].code - 96, move[3].toString().toInt())

            if (forwardMove in validForwardMoves) {
                pawns[pawns.indexOf(moveFrom)] = forwardMove

                when {
                    forwardMove.rank == 1 && forwardMove.color == "B" -> {
                        printChessboard(pawns)
                        println("Black Wins!")
                        return 1
                    }
                    forwardMove.rank == 8 && forwardMove.color == "W" -> {
                        printChessboard(pawns)
                        println("White Wins!")
                        return 1
                    }
                }

                if (forwardMove.rank - moveFrom.rank == 2 || forwardMove.rank - moveFrom.rank == -2) {
                    when (moveFrom.color) {
                        "W" -> enPassant = Pawn(moveFrom.color, moveFrom.file, moveFrom.rank + 1)
                        "B" -> enPassant = Pawn(moveFrom.color, moveFrom.file, moveFrom.rank - 1)
                    }
                } else {
                    enPassant = null
                }
            } else if (captureMove in validCaptureMoves) {
                if (captureMove != enPassant) {
                    pawns.remove(captureMove)
                    pawns[pawns.indexOf(moveFrom)] =
                        Pawn(if (captureMove.color == "W") "B" else "W", captureMove.file, captureMove.rank)
                    enPassant = null
                } else {
                    when (enPassant!!.color) {
                        "W" -> pawns.remove(Pawn(enPassant!!.color, enPassant!!.file, enPassant!!.rank + 1))
                        "B" -> pawns.remove(Pawn(enPassant!!.color, enPassant!!.file, enPassant!!.rank - 1))
                    }
                    pawns[pawns.indexOf(moveFrom)] = Pawn(if (captureMove.color == "W") "B" else "W", captureMove.file, captureMove.rank)
                    enPassant = null
                }
            } else {
                println("Invalid Input")
                continue@getMove
            }

            printChessboard(pawns)

            var wCount = 0
            var bCount = 0

            for (pawn in pawns) {
                if (pawn.color == "W") {
                    wCount++
                } else {
                    bCount++
                }
            }

            when {
                wCount == 0 -> {
                    println("Black Wins!")
                    return 1
                }
                bCount == 0 -> {
                    println("White Wins!")
                    return 1
                }
            }

            return 0
        }
    }

    while (true) {
        if (makeAMove(wPlayer) == 1) {
            return
        }
        if (makeAMove(bPlayer) == 1) {
            return
        }
    }
}

fun main() {
    println("Pawns-Only Chess")

    println("First Player's name:")
    val wPlayer = Player(readln(), "W")

    println("Second Player's name:")
    val bPlayer = Player(readln(), "B")

    val pawns = setPawns()

    printChessboard(pawns)

    playChess(wPlayer, bPlayer, pawns)

    println("Bye!")
}
