<template>
    <div id="app" class="app-container">
        <List :groups="groups" @add-to-cart="addToCart" />
        <Cart
            :cart="cart"
            :totalPrice="totalPrice"
            @remove-from-cart="removeFromCart"
            @increase-quantity="increaseQuantity"
            @decrease-quantity="decreaseQuantity"
        />
    </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue';
import List from './components/List.vue';
import Cart from './components/Cart.vue';

export default {
    components: {
        List,
        Cart
    },
    setup() {
        const groups = ref([]);
        const cart = ref([]);
        const exchangeRate = ref(50);

        const fetchData = async () => {
            const dataResponse = await fetch('/data.json');
            const dataJson = await dataResponse.json();
            console.log(dataJson);
            const namesResponse = await fetch('/names.json');
            const namesJson = await namesResponse.json();
            console.log(namesJson);
            const mappedGroups = mapDataToNames(dataJson.Value.Goods, namesJson);
            groups.value = mappedGroups;
        };

        const mapDataToNames = (dataGoods, namesData) => {
            const result = [];
            dataGoods.forEach((item) => {
                const group = namesData[item.G];
                const groupIndex = result.findIndex((g) => g.G === item.G);
                if (groupIndex === -1) {
                    result.push({
                        G: item.G,
                        name: group.G,
                        items: []
                    });
                }
                const product = group.B[item.T];
                result[result.length - 1].items.push({
                    ...item,
                    name: product.N,
                    priceInRubles: item.C * exchangeRate.value,
                    quantity: 1,
                    availableStock: item.P,
                    colorChange: null
                });
            });
            return result;
        };

        const addToCart = (item) => {
            const cartItem = cart.value.find(cartItem => cartItem.T === item.T);
            if (cartItem) {
                if (cartItem.availableStock > 0) {
                    cartItem.quantity++;
                    cartItem.availableStock--;
                }
            } else {
                cart.value.push({
                    ...item,
                    quantity: 1,
                    availableStock: item.P - 1
                });
            }
            const product = groups.value
                .find(group => group.G === item.G)
                .items.find(product => product.T === item.T);
            product.P--;
        };

        const removeFromCart = (item) => {
            const index = cart.value.indexOf(item);
            if (index !== -1) {
                cart.value.splice(index, 1);
            }
            const product = groups.value
                .find(group => group.G === item.G)
                .items.find(product => product.T === item.T);
            product.P += item.quantity;
        };

        const increaseQuantity = (item) => {
            if (item.availableStock > 0) {
                item.quantity++;
                item.availableStock--;
                const product = groups.value
                    .find(group => group.G === item.G)
                    .items.find(product => product.T === item.T);
                product.P--;
            }
        };

        const decreaseQuantity = (item) => {
            if (item.quantity > 1) {
                item.quantity--;
                item.availableStock++;
                const product = groups.value
                    .find(group => group.G === item.G)
                    .items.find(product => product.T === item.T);
                product.P++;
            }
        };

        const totalPrice = computed(() => {
            return cart.value.reduce((total, item) => total + item.priceInRubles * item.quantity, 0);
        });

        const updateExchangeRate = () => {
            const newRate = Math.floor(Math.random() * (80 - 20 + 1)) + 20;
            groups.value.forEach((group) => {
                group.items.forEach((item) => {
                    const oldPrice = item.priceInRubles;
                    item.priceInRubles = item.C * newRate;
                    if (item.priceInRubles > oldPrice) {
                        item.colorChange = 'red';
                    } else if (item.priceInRubles < oldPrice) {
                        item.colorChange = 'green';
                    } else {
                        item.colorChange = null;
                    }
                });
            });
            exchangeRate.value = newRate;
        };


        onMounted(() => {
            fetchData();
            setInterval(updateExchangeRate, 15000);
        });

        return {
            groups,
            cart,
            totalPrice,
            addToCart,
            removeFromCart,
            increaseQuantity,
            decreaseQuantity
        };
    }
};
</script>

<style scoped>
.app-container {
    display: flex;
    justify-content: space-between;
    width: 90%;
    padding: 20px;
    margin: 0 auto;
}
</style>
