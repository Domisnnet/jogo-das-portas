<template>
    <div class="gift">
        <img src="../assets/gift.png" alt="Gift" width="200" height="200" />
    </div>
</template>

<script>
export default {
    name: "GiftComponent",
};
</script>

<style>
.gift {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    z-index: 10;
}
</style>