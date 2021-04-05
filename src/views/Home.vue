<template>
  <div>
    <div class="container">
      <div class="search">
        <input type="text" v-model="search" placeholder="Busque um filme por nome, ano ou gênero...">
      </div>
      <div class="list">
        <film v-for="(row, id) in filteredListFilms" :value=row :tags=tags :key="id" />
      </div>
      <div class="pagination">
        <button v-for="(item, id) of paginator.total" :key="id" @click="setPage(item)" v-bind:class="item == paginator.current ? 'currentPage' : ''"><h2>{{item}}</h2></button>
      </div>
    </div>
  </div>
</template>

<script>
import film from '@/components/film'
export default {
  name: 'Home',
  data: function () {
    return {
      search: null,
      paginator: {
        current: 1,
        limit: 3,
        total: 0,
        pages: 0
      },
      listFilms: [],
      tags: []
    }
  },
  components:{
    film: film
  },
  watch:{
    search () {      
      this.paginator.current = 1
      this.paginator.total = this.filteredListFilms.length
    }
  },
  methods:{
    setPage(valor){
      this.paginator.current = valor
    },
  },
  computed: {
    filteredListFilms(){
      if(this.search !== null){
        return this.listFilms.filter((film) => {
          let cond1 = this.search.toLowerCase().split(' ').every(v => film.title.toLowerCase().includes(v))
          let cond2 = this.search.toLowerCase().split(' ').every(v => film.release_date.toLowerCase().includes(v))
          return (cond1 || cond2)
        }).filter((el, index) => {
          return ( index >= (this.paginator.current - 1) * this.paginator.limit
                        && index < this.paginator.current * this.paginator.limit);
            });
      }else{
        return this.listFilms.filter((el, index) => {
          return ( index >= (this.paginator.current - 1) * this.paginator.limit
                        && index < this.paginator.current * this.paginator.limit);
            });
      }
    },
  },
  beforeMount(){
    this.$http.get('https://api.themoviedb.org/3/movie/popular?api_key=6fff29ff14e17eb7f831e35c2702cb92&language=pt-BR').then(res => {
      this.listFilms = res.data.results  
      this.paginator.total = Math.ceil(this.listFilms.length / this.paginator.limit);    
      this.$http.get('https://api.themoviedb.org/3/genre/movie/list?api_key=6fff29ff14e17eb7f831e35c2702cb92&language=pt-BR').then(res => {
        this.tags = res.data.genres
      })
    })
  }
}
</script>
<style lang="scss" scoped>
  .container{
    margin:3rem;
    .search{
      input{
        background:#ebebeb;
        width:100%;
        border-radius:25px;
        padding:1rem 2rem;
        border:0;
        font-family: 'Abel', Helvetica, Arial, sans-serif;
        font-size:20px;
        outline: none;
        &::placeholder{
          color:#84aac2;
        }
      }
    }
    .list{
      display:flex;
        flex-wrap: wrap;
    }
    .pagination{
      display:flex;
      flex-wrap: wrap;
      justify-content: center;
      vertical-align: center;
      margin-top:2rem;
      
        button{
                    outline:none;
                    margin:1rem 10px;
                    background:none;
                    border:0;
                    h2{
                      font-weight: initial;
                    font-family: 'Lato', Helvetica, Arial, sans-serif;
                    color:#116193;
                    }
          &.currentPage{
                    border: 3px solid #116193;
                    background:#116193;
                    width:60px;
                    height:60px;
                    border-radius:100%;
                        padding: 0;
                    h2{
                        color:#00e8e4;
                        border: 4px solid #00e8e4;
                        width:100%;
                        height:100%;
                        display: flex;
                        font-size:2rem;
                        justify-content: center;
                        align-items: center;
                        border-radius:100%;
                        font-weight: initial;
                    }
                }
        }
    }
  }
</style>