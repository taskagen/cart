<template>
    <div class="cart">
        <h2>Корзина</h2>
        <CartItem
            v-for="cartItem in cart"
            :key="cartItem.T"
            :item="cartItem"
            @remove-from-cart="removeFromCart"
            @increase-quantity="increaseQuantity"
            @decrease-quantity="decreaseQuantity"
        />
        <div class="total-price">Общая стоимость: {{ totalPrice.toFixed(2) }} руб.</div>
    </div>
</template>

<script>
import CartItem from './CartItem.vue';

export default {
    components: {
        CartItem
    },
    props: {
        cart: Array,
        totalPrice: Number
    },
    emits: ['remove-from-cart', 'increase-quantity', 'decrease-quantity'],
    setup(props, { emit }) {
        const removeFromCart = (item) => {
            emit('remove-from-cart', item);
        };

        const increaseQuantity = (item) => {
            emit('increase-quantity', item);
        };

        const decreaseQuantity = (item) => {
            emit('decrease-quantity', item);
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
.cart {
    width: 30%;
    padding-top: 15px;
    padding-left: 20px;
    border: 1px solid lightgray;
    border-radius: 8px;
}

h2 {
    margin-top: 0;
}

.total-price {
    margin-top: 15px;
    font-weight: bold;
    font-size: 18px;
}
</style>

