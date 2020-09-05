<template>
    <div id="board">
        <Square
            v-for="index in gridSize * gridSize"
            :key="index"
            ref="squares"
            :squareId="index"
            :foodSymbol="
                foodSymbols[parseInt(Math.random() * foodSymbols.length)]
            "
            @elementDragged="setElementDragged"
            @elementReplaced="setElementReplaced"
            @endOfDrag="handleMove"
        />
    </div>
</template>

<script>
import Square from "./Square";

export default {
    name: "Board",
    data() {
        return {
            gridSize: 8,
            foodSymbols: ["🍔", "🍣", "🥞", "🍩", "🍪", "🍰"],
            symbolBeingDragged: "",
            squareIdBeingDragged: Number,
            symbolBeingReplaced: "",
            squareIdBeingReplaced: Number,
        };
    },
    props: {
        isStart: { type: Boolean, required: true },
    },
    methods: {
        checkRowForThree() {
            const nbSquares = this.gridSize * this.gridSize - 3;

            for (let i = 0; i < nbSquares; i++) {
                let rowOfThree = [i, i + 1, i + 2];

                const decidedColor = this.$refs.squares[i].symbol;
                const isBlank = this.$refs.squares[i].symbol === "";

                // Prevent check on 2 rows
                if ((i % this.gridSize).isInteger) {
                    continue;
                }

                if (
                    rowOfThree.every(
                        (index) =>
                            this.$refs.squares[index].symbol === decidedColor &&
                            !isBlank
                    )
                ) {
                    if (this.isStart) {
                        this.$emit("updateScore", 3);
                    }
                    rowOfThree.forEach((index) => {
                        this.$refs.squares[index].setSymbol("");
                    });
                }
            }
        },
        checkColumnForThree() {
            const nbSquares = this.gridSize * this.gridSize - this.gridSize * 2;

            for (let i = 0; i < nbSquares; i++) {
                let columnOfThree = [
                    i,
                    i + this.gridSize,
                    i + this.gridSize * 2,
                ];

                const decidedColor = this.$refs.squares[i].symbol;
                const isBlank = this.$refs.squares[i].symbol === "";

                if (
                    columnOfThree.every(
                        (index) =>
                            this.$refs.squares[index].symbol === decidedColor &&
                            !isBlank
                    )
                ) {
                    if (this.isStart) {
                        this.$emit("updateScore", 3);
                    }
                    columnOfThree.forEach((index) => {
                        this.$refs.squares[index].setSymbol("");
                    });
                }
            }
        },

        dropCandies() {
            for (let i = 0; i < 56; i++) {
                const squares = this.$refs.squares;

                if (squares[i + this.gridSize].symbol === "") {
                    squares[i + this.gridSize].setSymbol(squares[i].symbol);
                    squares[i].setSymbol("");
                }

                if (i <= this.gridSize && squares[i].symbol === "") {
                    squares[i].setSymbol(
                        this.foodSymbols[
                            parseInt(Math.random() * this.foodSymbols.length)
                        ]
                    );
                }
            }
        },

        setElementDragged(symbol, id) {
            this.symbolBeingDragged = symbol;
            this.squareIdBeingDragged = parseInt(id);
        },
        setElementReplaced(symbol, id) {
            this.symbolBeingReplaced = symbol;
            this.squareIdBeingReplaced = parseInt(id);

            const draggedSquare = this.$refs.squares[
                this.squareIdBeingDragged - 1
            ];
            draggedSquare.setSymbol(this.symbolBeingReplaced);

            const replacedSquare = this.$refs.squares[
                this.squareIdBeingReplaced - 1
            ];
            replacedSquare.setSymbol(this.symbolBeingDragged);
        },
        handleMove() {
            const validMoves = [
                this.squareIdBeingDragged - 1,
                this.squareIdBeingDragged - this.gridSize,
                this.squareIdBeingDragged + 1,
                this.squareIdBeingDragged + this.gridSize,
            ];

            const validMove = validMoves.includes(this.squareIdBeingReplaced);

            if (this.squareIdBeingReplaced && validMove) {
                this.squareIdBeingReplaced = null;
            } else if (this.squareIdBeingReplaced && !validMove) {
                this.$refs.squares[this.squareIdBeingReplaced - 1].setSymbol(
                    this.symbolBeingReplaced
                );
                this.$refs.squares[this.squareIdBeingDragged - 1].setSymbol(
                    this.symbolBeingDragged
                );
            } else {
                this.$refs.squares[this.squareIdBeingDragged - 1].setSymbol(
                    this.symbolBeingDragged
                );
            }
        },
    },
    components: {
        Square,
    },
    mounted() {
        setInterval(() => {
            this.checkRowForThree();
            this.checkColumnForThree();
            this.dropCandies();
        }, 100);
    },
};
</script>

<style scoped>
#board {
    background-color: #ffffffaa;
    display: grid;
    grid-template-columns: repeat(8, 1fr);
    border-radius: 2%;
    padding: 2vh;
}
</style>
