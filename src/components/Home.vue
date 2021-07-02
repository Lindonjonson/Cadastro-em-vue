<template>
<div class="container">
        <div class="row mt-5">
            <div class="col-12 border-bottom mb-5">
                <h2>Pessoas </h2>
            </div>

            <div class="col-4">

                <form action="">

                    <div class="form-group mr-1">
                        <label>Nome</label>
                        <input type="text" class="form-control" placeholder="Digite seu nome..." v-model="pessoa.nome">
                    </div>
                    <div class="form-group mr-1">
                        <label>Sobrenome</label>
                        <input type="text" class="form-control" placeholder="Digite seu sobrenome..." v-model="pessoa.sobrenome">
                    </div>


                    <div class="form-group mr-1">
                        <label>E-mail</label>
                        <input type="email" class="form-control" placeholder="digite seu e-mail..." v-model="pessoa.email">
                    </div>

                    <div class="form-group mr-1">
                        <label>telefone</label>
                        <input type="phone" class="form-control" placeholder="Telefone/Celular" v-model="pessoa.telefone">
                    </div>
                    <div v-if="!isEdit">
                    <label>Pessoa Juridica?</label>
                    <div class="form-check">
                         <input  class="form-check-input" type="radio" name="flexRadioDefault" id="true" v-model="pessoajuridica" >
                        <label class="form-check-label" for="flexRadioDefault1">Sim</label>
                    </div>
                    <div class="form-check">
                          <input  class="form-check-input" type="radio" name="flexRadioDefault" id="false"  v-model="pessoafisica">
                          <label class="form-check-label" for="flexRadioDefault2">Não</label>
                         
                    </div>
                    </div>
                    <div class="form-group mr-1" id="cpf">
                        <label>CPF</label>
                        <input type="email" maxlength="11"  class="form-control" placeholder="DIGITE SEU CPF SEM PONTO OU TRAÇO" v-model="pessoa.cpf">
                    </div>
                  
                    <div class="form-group mr-1" id="cnpj">
                        <label>CNPJ</label>
                        <input type="email" maxlength="14"  class="form-control" placeholder="DIGITE SEU CNPJ SEM PONTO OU TRAÇO" v-model="pessoa.cnpj">
                    </div>

                    <div class="form-group ml-1">
                    <br>
                        <button v-if="!isEdit" class="btn btn-lg btn-rounded btn-success" v-on:click.prevent="AddPessoa(pessoa)">Adicionar Pessoa</button>
                         <button v-if="isEdit" class="btn btn-lg btn-rounded btn-primary" v-on:click.prevent="atualizar(pessoa)">Atualizar</button>
                    </div>
                </form>
            </div>

            <div class="col-8 border-left">
                
                <div class="contact">
                        
                    <div class="col-12 mb-2" v-for="(pessoas) in pessoas" :key="pessoas.nome">

                        <div class="card">
                            <div class="card-body">
                                <h5 class="card-title"><b>Nome: </b> {{pessoas.nome}} {{pessoas.sobrenome}}</h5>
                                <p v-if="pessoas.cnpj"><b>CNPJ:</b> {{pessoas.cnpj}}</p>
                                <p v-else><b>CPF:</b> {{pessoas.cpf}}</p>
                                <p class="card-text"><b>TELEFONE: </b>{{pessoas.telefone}} <b>E-MAIL: </b>{{pessoas.email}} </p>

                                <a href="#" class="btn btn-sm btn-primary" @click.prevent="editar(pessoas)">Editar</a>
                                <a href="#" class="btn btn-sm btn-danger" @click.prevent="apagar(pessoas.id)">Apagar</a>
                            </div>

                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {

    data(){
        return {
          pessoas:[],
           pessoajuridica:'',
           pessoafisica: '',

           pessoa:{
             id: '',
             nome:'',
             sobrenome:'',
             email:'',
             telefone:'',
             pessoajuridica:'',
             cpf:'',
             cnpj:''

        },
        isEdit: false
    }
    },
   created() {
     this.pessoas = JSON.parse(localStorage.getItem('AppPessoas'));
   },
    mounted() {
       this.ocultarcpf();
       this.ocultarcnpj();
    },
    watch:{
      
      pessoajuridica(){ 
    
      this.exibircnpj();
      this.ocultarcpf();
   

      }, 
      pessoafisica(){
      
       this.exibircpf();
       this.ocultarcnpj();
      

      }
      },

      
      methods: {
        exibircpf() {
        document.getElementById("cpf").style.display = "block";
        },
        exibircnpj() {
        document.getElementById("cnpj").style.display = "block";
        },
        ocultarcnpj() {
        document.getElementById("cnpj").style.display = "none";
    },
        ocultarcpf() {
        document.getElementById("cpf").style.display = "none";
    },




      AddPessoa(pessoa){
        if(pessoa.cpf){
          pessoa.pessoajuridica = false
          pessoa.id = new Date().getTime();
      let pessoas = localStorage.getItem('AppPessoas');
        
        if(pessoas){
          pessoas = JSON.parse(pessoas);
          pessoas.push(pessoa);
        }else{
          pessoas = [pessoa];
        }
        this.pessoas = pessoas;

      localStorage.setItem('AppPessoas', JSON.stringify(pessoas))
      location.reload()
      alert('cadastrado')
        }else{
          pessoa.pessoajuridica = true
          pessoa.id = new Date().getTime();
      let pessoas = localStorage.getItem('AppPessoas');
        
        if(pessoas){
          pessoas = JSON.parse(pessoas);
          pessoas.push(pessoa);
        }else{
          pessoas = [pessoa];
        }
        this.pessoas = pessoas;

      localStorage.setItem('AppPessoas', JSON.stringify(pessoas))
      location.reload()
      alert('cadastrado')
        }
        
        
      

      },
      apagar(id){
       var result = confirm("Tem certeza que deseja apagar ?");
       if (result == false) return;
         let pessoas = localStorage.getItem('AppPessoas');
         pessoas = JSON.parse(pessoas);
         pessoas = pessoas.filter((pessoa) => {
           return pessoa.id != id;
         });
         this.pessoas = pessoas;
        localStorage.setItem('AppPessoas', JSON.stringify(pessoas))
        alert('apagado'+id)
      },
      editar(pessoa){
       this.pessoa = pessoa;
       alert(pessoa.pessoajuridica);
       if(pessoa.pessoajuridica == false){
       this.exibircpf();
       this.ocultarcnpj();
       
       }else{
      this.exibircnpj();
      this.ocultarcpf();
       }
       this.isEdit = true;
      },
      atualizar(pessoa){
       let pessoas = this.pessoas.map(pessoamap =>{
         if(pessoamap.id ==pessoa.id ){
           return pessoa;
         }
         return pessoamap;
       });

       this.pessoas = pessoas;
       this.isEdit = false;
     localStorage.setItem('AppPessoas', JSON.stringify(pessoas))
     location.reload();

      }
      
 }}
</script>
