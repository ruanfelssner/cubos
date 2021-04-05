<template>
  <div class="film">
      <div class="thumb">
          <img @click="$router.push('/film/'+value.id)" :src="'https://www.themoviedb.org/t/p/w600_and_h900_bestv2/'+value.backdrop_path" alt="">
      </div>
      <div class="main">
          <div class="titulo">
              <h1 @click="$router.push('/film/'+value.id)">{{ value.title }}</h1>              
                <div class="porcentagem">
                    <h2>{{ value.vote_average*10 }}%</h2>
                </div>
          </div>
          <div class="data">              
              <p>{{moment(value.release_date).format('DD/MM/YYYY')}}</p>
          </div>
          <div class="descricao">
              <p>{{ value.overview }}</p>
          </div>
          <div class="tags">
              <button v-for="(item, id) of listTags" :key="id">{{ item.name }}</button>
          </div>
      </div>
  </div>
</template>

<script>
export default {
    props: ['value', 'tags'],
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
    }
}
</script>

<style lang="scss" scoped>
.film{
    display:flex;
    flex-wrap: wrap;
    margin-top:3rem;
    .thumb{
        width:25%;
        background:#ebebeb;
        img{
            height: auto;
            max-width:100%;
            display: block;
        }
    }
    .main{
        width:75%;
        background:#ebebeb;
        position: relative;
        .titulo{
            background:#116193;
            padding-left:140px;
            width: 100%;
            position: relative;
                h1{
                    color:#00e8e4;
                    font-weight: initial;
                    font-size:45px;
                    text-align: left;                    
                    padding-top: 20px;
                    min-height: 40px;
                    padding-bottom: 20px;
                }
                
                .porcentagem{
                    width:80px;
                    height:80px;
                    border: 3px solid #116193;
                    border-radius:100%;
                    position: absolute;
                    left: 35px;
                    bottom: -35px;
                    background:#116193;
                    h2{
                        color:#00e8e4;
                        border: 4px solid #00e8e4;
                        width:100%;
                        height:100%;
                        display: flex;
                        justify-content: center;
                        align-items: center;
                        border-radius:100%;
                        font-weight: initial;
                    }
                }
        }
        .data{
            padding-left:140px;
            text-align: left;
            p{
                margin:10px 0;
                font-size: 1.4rem;
                font-weight: initial;
                font-family: 'Lato', Helvetica, Arial, sans-serif;
            }
        }
        .descricao {
            padding:0 2rem;
            p{
                font-size: 1.2rem;
                text-align: justify;
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
    }
}
@media only screen and (max-width: 600px) {
  .thumb, .main {
    width:100% !important;
  }
}
</style>