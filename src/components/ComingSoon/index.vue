<template>
    <div class="movie_body">
              <Loading v-if="isLoading"/>

        <Scroller>
             <ul>
            <!-- <li>
                <div class="pic_show"><img src="/images/movie_1.jpg"></div>
                <div class="info_list">
                    <h2>无名之辈</h2>
                    <p><span class="person">17746</span> 人想看</p>
                    <p>主演: 陈建斌,任素汐,潘斌龙</p>
                    <p>2018-11-30上映</p>
                </div>
                <div class="btn_pre">
                    预售
                </div>
            </li> -->
             <li v-for="item in comingList" :key="item.id">
                <div class="pic_show" @tap="handleToDetail(item.id)"><img :src="item.img | setWH('128.180')"></div>
                <div class="info_list">
                    <h2 @tap="handleToDetail(item.id)">{{item.nm}} <img height="30px" v-if="item.version" src="@/assets/IMAX.png"></h2>
                    <p><span class="person">{{item.wish}}</span> 人想看</p>
                    <p>主演:{{item.star}}</p>
                    <p>{{item.rt}}</p>
                </div>
                <div class="btn_pre">
                    预售
                </div>
            </li>
        </ul>
        </Scroller>
       
    </div>
</template>

<script>
// import { log } from 'util';
export default {
    name:'ComingSoon',
    data(){
        return {
            comingList:[],
            isLoading:true,
            prevCityId:-1
        }
    },
    activated(){
        var cityId=this.$store.state.city.id;
        if(this.prevCityId===cityId){return;}
        this.isLoading=true;
        this.axios.get('/api/movieComingList?cityId=10').then((res)=>{
            var msg=res.data.msg;
            if(msg === 'ok'){
                this.comingList = res.data.data.comingList;
                this.isLoading =false;
                this.prevCityId=cityId;
           }
        })
        
    },
    methods:{
        handleToDetail(movieId){
            this.$router.push('/movie/detail/2/'+movieId);
        }
    }
            // console.log(comList);

}
    // console.log(comingList);
    
</script>

<style scoped>
#content .movie_menu{ width: 100%; height: 45px; border-bottom:1px solid #e6e6e6; display: flex; justify-content:space-between; align-items:center; background:white; z-index:10;}
.movie_menu .city_name{ margin-left: 20px; height:100%; line-height: 45px;}
.movie_menu .city_name.active{ color: #ef4238; border-bottom: 2px #ef4238 solid;}
.movie_menu .hot_swtich{ display: flex; height:100%; line-height: 45px;}
.movie_menu .hot_item{ font-size: 15px; color:#666; width:80px; text-align:center; margin:0 12px; font-weight:700;}
.movie_menu .hot_item.active{ color: #ef4238; border-bottom: 2px #ef4238 solid;}
.movie_menu .search_entry{ margin-right:20px; height:100%; line-height: 45px;}
.movie_menu .search_entry.active{ color: #ef4238; border-bottom: 2px #ef4238 solid;}
.movie_menu .search_entry i{  font-size:24px; color:red;}
</style>
