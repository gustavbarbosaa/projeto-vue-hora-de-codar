<template>
  <div id="burguer-table">
    <div>
        <Message :msg="msg" v-show="msg"/>
        <div id="burguer-table-heading">
            <div class="order-id">#:</div>
            <div>Cliente:</div>
            <div>Pão:</div>
            <div>Opcionais:</div>
            <div>Ações:</div>
        </div>
        <div id="burguer-table-rows">
            <div class="burguer-table-row" v-for="burger in burgers" :key="burger.id">
                <div class="order-number">{{burger.id}}</div>
                <div>{{burger.nome}}</div>
                <div>{{burger.pao}}</div>
                <div>{{burger.carne}}</div>
                <div>
                    <ul>
                        <li v-for="(opcional, index) in burger.opcionais" :key="index">{{opcional}}</li>
                    </ul>
                </div>
                <div>
                    <select name="status" class="status" @change="updateBurger($event, burger.id)">
                        <option value="">Selecione</option>
                        <option :value="sta.tipo" v-for="sta in status" :key="sta.id" :selected="burger.status == sta.tipo">{{sta.tipo}}</option>
                    </select>
                    <button class="delete-btn" @click="deleteBurger(burger.id)">Cancelar</button>
                </div>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
import Message from './Message.vue';

export default {
    name: 'Dashboard',
    data() {
        return {
            burgers: null,
            burgers_id: null,
            status: [],
            msg: null,
        }
    },
    methods: {
        async getPedidos () {
            const req = await fetch('http://localhost:3000/burgers');

            const data = await req.json();

            this.burgers = data;

            this.getStatus();
        },
        async getStatus() {
            const req = await fetch('http://localhost:3000/status');

            const data = await req.json();

            this.status = data;
        },
        async deleteBurger(id) {
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: 'DELETE',
            });

            const res = await req.json();

            this.msg = `Pedido deletado!`;

            setTimeout(() => this.msg = '', 3000);

            this.getPedidos();
        },
        async updateBurger(event, id) {
            const option = event.target.value;

            const dataJson = JSON.stringify({status: option});

            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: 'PATCH',
                headers: {'Content-Type': 'application/json'},
                body: dataJson
            });

            const res = await req.json();

            this.msg = `Pedido n°${res.id} atualizado com sucesso!`;
            
            setTimeout(() => this.msg = '', 3000);

        }
    },
    mounted() {
        this.getPedidos();
    },
    components: {
        Message
    }
}
</script>

<style scoped>
    #burguer-table {
        max-width: 1200px;
        margin: 0 auto;
    }

    #burguer-table-heading,
    #burguer-table-rows,
    .burguer-table-row {
        display: flex;
        flex-wrap: wrap;
    }

    #burguer-table-heading {
        font-weight: bold;
        padding: 12px;
        border-bottom: 3px solid #333;
    }

    .burguer-table-row {
        width: 100%;
        padding: 12px;
        border-bottom: 1px solid #ccc;
    }

    #burguer-table-heading div,
    .burguer-table-row div {
        width: 19%;
    }

    #burguer-table-heading .order-id,
    .burguer-table-row .order-number {
        width: 5%;
    }

    select {
        padding: 12px 6px;
        margin-right: 12px;
    }

    .delete-btn {
        background-color: #222;
        color: #fcba03;
        font-weight: bold;
        font-size: 16px;
        border: 2px solid #222;
        padding: 10px;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s;
    }

    .delete-btn:hover {
        background-color: transparent;
        color: #222;
    }

</style>