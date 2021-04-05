<template>
  <div class="film">
      
    <div class="titulo">
        <h1>{{ value.title }}</h1>  
        <div class="data">              
              <p>{{moment(value.release_date).format('DD/MM/YYYY')}}</p>
          </div>   
    </div>
      <div class="main">
          
          <div class="descricao">
              <h2>Sinopse</h2>
              <hr />
              <p>{{ value.overview }}</p>
              
              <h2>Informações</h2>
              <hr />
              <div class="infos">
                  <div class="info">
                      <h3>Situação</h3>
                      <p>{{ value.status }}</p>
                  </div>
                  <div class="info">
                      <h3>Idioma</h3>
                      <p>{{ value.spoken_languages[0].name }}</p>
                  </div>
                  <div class="info">
                      <h3>Duração</h3>
                      <p>{{ value.runtime }} min</p>
                  </div>
                  <div class="info">
                      <h3>Orçamento</h3>
                      <p>${{ value.budget }}</p>
                  </div>
                  <div class="info">
                      <h3>Receita</h3>
                      <p>${{ value.revenue }}</p>
                  </div>
                  <div class="info">
                      <h3>Lucro</h3>
                      <p>${{ parseFloat(value.revenue - value.budget)}}</p>
                  </div>
              </div>
          </div>
          <div class="tags">
              <button v-for="(item, id) of value.genres" :key="id">{{ item.name }}</button>
          </div>
                   
          <div class="porcentagem">
              <h2>{{ value.vote_average*10 }}%</h2>
          </div>
      </div>
      
      <div class="thumb">
          <img :src="'https://www.themoviedb.org/t/p/w600_and_h900_bestv2/'+value.backdrop_path" alt="">
      </div>
  </div>
</template>

<script>
export default {
    data: function(){
        return {
            value: {}
        }
    },
    computed: {
        listTags () {
            return this.tags.filter((tag) => { 
                for(let item of this.value.genre_ids){
                    if(tag.id == item){
                        return tag
                    }
                }              
            })
        }
    },
    beforeMount () {
        this.$http.get('https://api.themoviedb.org/3/movie/'+this.$route.params.id+'?api_key=6fff29ff14e17eb7f831e35c2702cb92&language=pt-BR').then(res => {
            this.value = res.data
        })
    }
}
</script>

<style lang="scss" scoped>
.film{
    display:flex;
    flex-wrap: wrap;
    margin-top:3rem;
    .thumb{
        width:35%;
        background:#f2f2f2;
        img{
            height: auto;
            max-width:100%;
            display: block;
        }
    }
    
        .titulo{
            background:#e6e6e6;
            padding-left:40px;
            padding-right:40px;
            width: 100%;
            display:flex;
            flex-wrap: wrap;
            justify-content: space-between;
            text-align: center;
                h1{
                    color:#116193;
                    font-weight: initial;
                    font-size:45px;
                    text-align: left;                    
                    padding-top: 20px;
                    min-height: 40px;
                    padding-bottom: 20px;
                }
        
        .data{
            text-align: left;
            display:flex;
            justify-content: center;
            align-items: center;
            p{
                margin:10px 0;
                font-size: 1.4rem;
                font-weight: initial;
                font-family: 'Lato', Helvetica, Arial, sans-serif;
            }
        }
                
        }
    .main{
        width:65%;
        background:#f2f2f2;
        position: relative;
        padding-bottom:130px;
        .descricao {
            padding:0 2rem;
                h2{
                    
                        text-align:left;
                        margin-top:2rem;
                        font-weight: initial;
                        color:#116193;
                }
                hr{
                    border-top: 1px solid #00e8e4;
                }
            p{
                font-size: 1.2rem;
                text-align: justify;
            }
            .infos{
                display:flex;
                flex-wrap: wrap;
                justify-content: space-between;
                text-align: center;
                align-items: stretch;
                .info{
                    flex-grow: 1;
                    margin-top:1rem;
                    margin-bottom:3rem;
                    h3{
                        color:#116193;
                        font-weight: initial;
                    }
                    p{
                        text-align: center;
                        padding:0;
                        margin:0;
                    }
                }
            }
        }
        .tags{
            display: flex;
            flex-wrap: wrap;
            padding:0 2rem;
            button{
                background:#FFF;
                border:1px solid #116193;
                border-radius:30px;
                font-family: 'Lato', Helvetica, Arial, sans-serif;
                padding:7px 15px;
                color:#116193;
                font-size:1rem;
                margin-right:1rem;
                outline: none;
                margin-bottom:1rem;
            }
        }
        
                .porcentagem{
                    width:160px;
                    height:160px;
                    border: 6px solid #116193;
                    border-radius:100%;
                    position: absolute;
                    right: 30px;
                    bottom: 30px;
                    background:#116193;
                    h2{
                        color:#00e8e4;
                        border: 8px solid #00e8e4;
                        width:100%;
                        height:100%;
                        font-size:3rem;
                        display: flex;
                        justify-content: center;
                        align-items: center;
                        border-radius:100%;
                        font-weight: initial;
                    }
                }
    }
}
@media only screen and (max-width: 600px) {
  .thumb, .main {
    width:100% !important;
  }
}
</style>