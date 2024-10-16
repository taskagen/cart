<template>
    <div class="product-list">
        <div v-for="group in groups" :key="group.G" class="group">
            <h2>{{ group.name }}</h2>
            <ListItem
                v-for="item in group.items"
                :key="item.T"
                :item="item"
                @add-to-cart="addToCart"
            />
        </div>
    </div>
</template>

<script>
import ListItem from './ListItem.vue';

export default {
    components: {
        ListItem
    },
    props: {
        groups: Array
    },
    emits: ['add-to-cart'],
    setup(props, { emit }) {
        const addToCart = (item) => {
            emit('add-to-cart', item);
        };

        return {
            addToCart
        };
    }
};
</script>

<style scoped>
.product-list {
    width: 70%;
    padding-right: 20px;
}

.group {
    margin-bottom: 20px;
    padding: 15px;
    border: 1px solid lightgray;
    border-radius: 8px;
}

h2 {
    margin-bottom: 10px;
}
</style>
