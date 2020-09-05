<template>
    <span
        class="emoji"
        role="img"
        :aria-label="label ? label : ''"
        :aria-hidden="label ? 'false' : 'true'"
    >
        {{ symbol }}
    </span>
</template>

<script>
export default {
    name: "Emoji",
    props: {
        label: {
            type: String,
        },
        symbol: {
            type: String,
            required: true,
        },
    },
};
</script>

<style scoped>
.emoji {
    font-size: 5vh;
}
</style>
