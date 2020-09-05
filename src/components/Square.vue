<template>
    <div
        class="square"
        draggable="true"
        @dragstart="dragStart"
        @dragend="dragEnd"
        @dragover.prevent="dragOver"
        @dragenter.prevent="dragEnter"
        @drop="dragDrop"
    >
        <Emoji :symbol="symbol" />
    </div>
</template>

<script>
import Emoji from "./Emoji";

export default {
    name: "Square",
    data() {
        return {
            initialization: true,
            symbol: "",
        };
    },
    props: {
        squareId: {
            type: Number,
            required: true,
        },
        foodSymbol: {
            type: String,
            required: true,
        },
    },
    components: {
        Emoji,
    },
    methods: {
        setSymbol(symbol) {
            this.symbol = symbol;
        },
        dragStart() {
            this.$emit("elementDragged", this.symbol, this.squareId);
        },
        dragEnd() {
            this.$emit("endOfDrag", this.squareId);
        },
        dragOver() {},
        dragEnter() {},
        dragDrop() {
            this.$emit("elementReplaced", this.symbol, this.squareId);
        },
    },
    mounted() {
        this.symbol = this.foodSymbol;
        this.initialization = false;
    },
};
</script>

<style scoped>
.square {
    display: grid;
    place-items: center;
}
</style>
