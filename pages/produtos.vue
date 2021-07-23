<template>
    <div>
        <v-form v-model="valid">
            <v-container>
                <v-row>
                    <v-col
                    cols="12"
                    md="4"
                    >
                    <v-text-field
                        v-model="nome"
                        :rules="nomeRules"
                        :counter="20"
                        label="Nome"
                        required
                    ></v-text-field>
                    </v-col>

                    <v-col
                    cols="12"
                    md="4"
                    >
                    <v-text-field
                        v-model="validade"
                        :rules="validadeRules"
                        :counter="2"
                        label="Validade (Meses)"
                        required
                    ></v-text-field>
                    </v-col>

                    <v-col
                    cols="12"
                    md="4"
                    >
                    <v-text-field
                        v-model="preco"
                        :rules="precoRules"
                        label="Preço"
                        type="number"
                        required
                    ></v-text-field>
                    </v-col>
                </v-row>
                <v-btn
                :disabled="!valid"
                color="success"
                class="mr-4"
                @click="insereDados"
                >
                Salvar
                </v-btn>
            </v-container>
        </v-form>
        <tableproducts :prods="produtos"></tableproducts>
        <nuxt-link to="/" style="text-decoration:none"><v-btn
        depressed
        color="error"
        style="margin-top: 5%"
        >
        Início
        </v-btn></nuxt-link>
    </div>
</template>

<script>

export default ({
    data() {
        return{
            produtos: null,
            url: "https://apiproductsgama.herokuapp.com/",
            valid: false,
            nome: '',
            validade: '',
            nomeRules: [
                v => !!v || 'Nome é obrigatório',
                v => v.length <= 20 || 'Nome não pode conter mais que 20 caracteres',
            ],
            validadeRules: [
                v => !!v || 'Validade é obrigatória',
                v => v.length <= 2 || 'Validade não pode conter mais que 2 caracteres',
            ],
            preco: '',
            precoRules: [
                v => !!v || 'Preço é obrigatório'                
            ],
        }
    },

    methods:{
        async buscaDados(){
            try{
                const request = await this.$axios.get(this.url+'produtos');
                this.produtos = request.data;
                console.log(this.produtos)
            } catch{
                console.log('Error');
            }
        },

        async insereDados(){
            try{
                const request = await this.$axios.post(this.url+'addProd',{nome: this.nome , validade: this.validade, preco: this.preco});
                this.nome = '';
                this.validade = '';
                this.preco = '';

                await this.buscaDados();
                alert('Dados inseridos com sucesso!');
            } catch{
                console.log('Error')
            }
        },

       
    },

    created(){
        this.buscaDados();
    }
})
</script>
