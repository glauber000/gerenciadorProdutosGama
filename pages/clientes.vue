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
                        :counter="26"
                        label="Nome"
                        required
                    ></v-text-field>
                    </v-col>

                    <v-col
                    cols="12"
                    md="4"
                    >
                    <v-text-field
                        v-model="idade"
                        :rules="idadeRules"
                        :counter="3"
                        label="Idade"
                        required
                    ></v-text-field>
                    </v-col>

                    <v-col
                    cols="12"
                    md="4"
                    >
                    <v-text-field
                        v-model="endereco.cep"
                        :rules="cepRules"
                        :counter="9"
                        label="Cep"
                        required
                    ></v-text-field>
                    </v-col>
                </v-row>
                <v-row>
                    <v-col
                    cols="12"
                    md="4"
                    >
                    <v-text-field
                        v-model="endereco.logradouro"
                        :rules="logradouroRules"
                        :counter="30"
                        label="Logradouro"
                        required
                    ></v-text-field>
                    </v-col>

                    <v-col
                    cols="12"
                    md="4"
                    >
                    <v-text-field
                        v-model="endereco.bairro"
                        :rules="bairroRules"
                        :counter="20"
                        label="Bairro"
                        required
                    ></v-text-field>
                    </v-col>

                    <v-col
                    cols="12"
                    md="4"
                    >
                    <v-text-field
                        v-model="endereco.cidade"
                        :rules="cidadeRules"
                        :counter="20"
                        label="Cidade"
                        required
                    ></v-text-field>
                    </v-col>
                </v-row>
                <v-row>
                    <v-col
                    cols="12"
                    md="4"
                    >
                    <v-text-field
                        v-model="endereco.uf"
                        :rules="ufRules"
                        :counter="2"
                        label="UF"
                        required
                    ></v-text-field>
                    </v-col>

                    <v-col
                    cols="12"
                    md="4"
                    >
                    <v-text-field
                        v-model="endereco.numero"
                        :rules="numeroRules"
                        :counter="10"
                        label="Número"
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
        <tableclientes :cln="clientes"></tableclientes>
        
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
            clientes: null,
            url: "https://apiproductsgama.herokuapp.com/",
            nome: '',
            idade: '',
            endereco:{
                logradouro: '',
                cidade: '',
                uf: '',
                numero: '',
                cep: '',
                bairro:'',
            },
            valid: false,
            nomeRules: [
                v => !!v || 'Nome é obrigatório',
                v => v.length <= 26 || 'Nome não pode conter mais que 26 caracteres',
            ],
            idadeRules: [
                v => !!v || 'Idade é obrigatória',
                v => v.length <= 3 || 'Idade não pode conter mais que 3 caracteres',
            ],
            cepRules: [
                v => !!v || 'Cep é obrigatório',
                v => /.+\d-.+\d.+/.test(v) || 'Cep Inválido',
                v => v.length <= 9 || 'Cep não pode conter mais que 9 caracteres',
                v => v.length >= 9 || 'O Cep precisa conter ao menos 9 caracteres digite com o "-"',
            ],
            logradouroRules: [
                v => !!v || 'Logradouro é obrigatório',
                v => v.length <= 30 || 'Logradouro não pode conter mais que 30 caracteres',
            ],
            bairroRules: [
                v => !!v || 'Bairro é obrigatório',
                v => v.length <= 20 || 'Bairro não pode conter mais que 20 caracteres',
            ],
            cidadeRules: [
                v => !!v || 'Cidade é obrigatório',
                v => v.length <= 20 || 'Cidade não pode conter mais que 20 caracteres',
            ],
            ufRules: [
                v => !!v || 'UF é obrigatório',
                v => v.length <= 2 || 'UF não pode conter mais que 2 caracteres',
            ],
            numeroRules: [
                v => !!v || 'Número é obrigatório',
                v => v.length <= 10 || 'Número não pode conter mais que 10 caracteres',
                v => /\d/.test(v) || 'Digite apenas números',
            ],
        }
    },

    methods:{
        async buscaDados(){
            try{
                const request = await this.$axios.get(this.url+'clientes');
                this.clientes = request.data;
            } catch{
                console.log('Error');
            }
        },

        async insereDados(){
            try{
                const request = await this.$axios.post(this.url+'addClin',{nome: this.nome, idade: this.idade, endereco: this.endereco})
                this.nome = '';
                this.idade = '';
                this.endereco.logradouro = '';
                this.endereco.cidade = '';
                this.endereco.uf = '';
                this.endereco.numero = '';
                this.endereco.cep = '';
                this.endereco.bairro = '';

                await this.buscaDados();
                alert('Dados Inseridos com Sucesso!');
            } catch{
                console.log('Error');
            }
        }
    },

    created(){
        this.buscaDados();
    }
})

</script>
