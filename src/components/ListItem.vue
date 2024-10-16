<template>
    <div class="item">
        <span class="item-name">{{ item.name }}</span>
        <span class="item-price" :style="{ color: item.colorChange ? item.colorChange : 'black' }">
      {{ item.priceInRubles.toFixed(2) }} руб.
    </span>
        <span class="item-stock">Остаток: {{ item.P }}</span>
        <button
            :disabled="item.P === 0"
            @click="addToCart(item)"
            class="add-to-cart-button"
        >
            {{ item.P === 0 ? 'Нет в наличии' : 'Добавить в корзину' }}
        </button>
    </div>
</template>

<script>
export default {
    props: {
        item: Object
    },
    emits: ['add-to-cart'],
    setup(props, { emit }) {
        const addToCart = (item) => {
            if (item.P > 0) {
                emit('add-to-cart', item);
            }
        };

        return {
            addToCart
        };
    }
};
</script>

<style scoped>
.item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
    border-bottom: 1px solid lightgray;
}

.item-name {
    width: 40%;
}

.item-price {
    width: 20%;
    text-align: right;
}

.item-stock {
    width: 15%;
    text-align: center;
}

button.add-to-cart-button {
    width: 20%;
    background-color: green;
    color: white;
    border: none;
    padding: 8px 12px;
    cursor: pointer;
    border-radius: 4px;
}

button.add-to-cart-button:disabled {
    background-color: grey;
    cursor: not-allowed;
}

button.add-to-cart-button:hover:enabled {
    background-color: darkgreen;
}
</style>
