<template>
  <div class="product">
      <HeaderShayna/>
            <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more text-left">
                       <router-link to="/"><i class="fa fa-home"></i> Home </router-link>
                        <span>Detail</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="row">
                        <div class="col-lg-6">
                            <div class="product-pic-zoom">
                                <img class="product-big-img" :src= "gambar_default" alt="" />
                            </div>
                          <div class="product-thumbs" v-if="ProductDetails.galleries.length > 0">
                  <carousel :dots="false" :nav="false" class="product-thumbs-track ps-slider">
                    <div
                      v-for="ss in ProductDetails.galleries"
                      :key="ss.id"
                      class="pt"
                      @click="changeImage(ss.photo)"
                      :class="ss.photo == gambar_default ? 'active' : '' "
                    >
                      <img :src="ss.photo" alt />
                    </div>
                  </carousel>
                            </div>
                        </div>
                        <div class="col-lg-6">
                            <div class="product-details text-left">
                                <div class="pd-title">
                                    <span>{{ProductDetails.type }}</span>
                                    <h3>{{ProductDetails.name }}</h3>
                                </div>
                                <div class="pd-desc">
                                    <p v-html="ProductDetails.desc"></p>
                                    <h4>{{ProductDetails.price | currency}}</h4>
                                </div>
                                <div class="quantity">
                                       <router-link to="/cart">
                                <a @click="saveKeranjang(ProductDetails.id, ProductDetails.name, ProductDetails.price, ProductDetails.galleries[0].photo)" href="#" class="primary-btn pd-cart">Add To Cart</a>
                                         </router-link>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Product Shop Section End -->
    <!-- <related> -->
    <RelatedShayna/>
    <!-- < end related> -->


     <FooterShayna/>
  
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import HeaderShayna from "@/components/HeaderShayna.vue"
import FooterShayna from  "@/components/FooterShayna.vue"
import RelatedShayna from  "@/components/RelatedShayna.vue"
import carousel from 'vue-owl-carousel';
import axios from 'axios';
export default{
  name: 'product',
  components: {
   HeaderShayna,
   RelatedShayna,
   FooterShayna,
    carousel,

  },
  data(){
      return{
          gambar_default:"",
          photo_thumbs:[],
          ProductDetails:[],
          keranjangUser:[]
      }
  },
  methods: {
      changeImage(urLImage){
          this.gambar_default = urLImage;
        
      },
       setDataPicture(data) {
      // replace object productDetails dengan data dari API
      this.ProductDetails = data;
      // replace value gambar default dengan data dari API (galleries)
      this.gambar_default = data.galleries[0].photo;
    },
     saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct) {
      var productStored = {
        "id": idProduct,
        "name": nameProduct,
        "price": priceProduct,
        "photo": photoProduct
      }
         this.keranjangUser.push(productStored);
          const parsed = JSON.stringify(this.keranjangUser);
         localStorage.setItem('keranjangUser', parsed);
    }
  },
   mounted(){
    if (localStorage.getItem('keranjangUser')) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
      } catch(e) {
        localStorage.removeItem('keranjangUser');
      }
    }
    axios
        .get("http://127.0.0.1:8000/api/products",{
            params: {
                    id:this.$route.params.id
            }
        })
        .then(res => this.setDataPicture(res.data.data))
        .catch(err=>console.log(err));
    }
}
</script>
<style scoped>
.product-thumbs .pt{
    margin-right: 14px;
}
</style>

