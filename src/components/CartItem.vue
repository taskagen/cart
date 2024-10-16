<template>
    <div class="cart-item">
        <span>
            <span class="cart-item-name">{{ item.name }} - </span>
            <span class="cart-item-price">{{ item.priceInRubles.toFixed(2) }} руб.</span>
        </span>
        <span class="cart-item-quantity">
            <button @click="decreaseQuantity(item)" :disabled="item.quantity === 1">-</button>
            <span>{{ item.quantity }}</span>
            <button @click="increaseQuantity(item)" :disabled="item.availableStock === 0">+</button>
            <button class="remove-button" @click="removeFromCart(item)">Удалить</button>
        </span>
    </div>
</template>

<script>
export default {
    props: {
        item: Object
    },
    emits: ['remove-from-cart', 'increase-quantity', 'decrease-quantity'],
    setup(props, { emit }) {
        const removeFromCart = (item) => {
            emit('remove-from-cart', item);
        };

        const increaseQuantity = (item) => {
            if (item.availableStock > 0) {
                emit('increase-quantity', item);
            }
        };

        const decreaseQuantity = (item) => {
            if (item.quantity > 1) {
                emit('decrease-quantity', item);
            }
        };

        return {
            removeFromCart,
            increaseQuantity,
            decreaseQuantity
        };
    }
};
</script>

<style scoped>
.cart-item {
    display: flex;
    justify-content: space-between;
    flex-direction: column;
    padding: 10px;
    border-bottom: 1px solid lightgray;
}

.cart-item-name {
    width: 70%;
}

.cart-item-price {
    width: 30%;
    text-align: right;
}

.cart-item-quantity {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: start;
    width: 100%;
    margin-top: 10px;
}

button {
    margin: 0 5px;
}

button.remove-button {
    background-color: red;
    color: white;
    border: none;
    padding: 8px 12px;
    cursor: pointer;
    border-radius: 4px;
}

button.remove-button:hover {
    background-color: darkred;
}
</style>
