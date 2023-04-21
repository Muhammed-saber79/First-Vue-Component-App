<template>
    <div class="container" id="mainApp">
        <div class="row">
            <div class="bg-black text-light d-flex justify-content-between align-items-baseLine fixed-top mb-3 p-3 fs-4">
                <a href="#" style="text-decoration: none;color:gold" @click.prevent="hideCart">Books</a>
                <div>
                    <small class="text-grey">{{wishList.books.length}} <span v-if="wishList.books.length == 1">item</span> <span v-else>items</span> with total price {{formatter(calcTotalPrice())}}</small>&nbsp;&nbsp;
                    <button class="btn btn-info" @click="showCart">Go To Wish List</button>
                </div>
            </div>
        </div>
        <!-- End Of Top Nav -->

        <div class="row" v-if="isBooksShown" style="margin-top: 3rem;margin-bottom:5rem !important;">
            <div  class="col-sm-12 col-md-6 col-lg-4" v-for="book in books" :key="book.id" :title="book.author"> 
                <div class="card my-3">
                    <img class="card-img-top" :src="book.image" alt="Title">
                    <div class="card-body">
                        <h4 class="card-title">{{book.title}}</h4>
                        <hr>
                        <div class="d-flex justify-content-between align-baseline m-auto p-2">
                            <div class="left">
                                <h5>{{book.id}}</h5>
                                <h5 :class="book.pages > 50? 'more':'less'">{{book.pages}}</h5>
                                <h5>{{book.category}}</h5>
                            </div>
                            <div class="right">
                                <h5>{{book.author}}</h5>
                                <h5>{{formatter(book.price)}}</h5>
                                <button :class="checkAdded(book)? 'btn btn-secondary':'btn btn-primary'" :disabled="checkAdded(book)" @click="addToWishList(book)">Add To Check List</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <!-- End Of Books -->

        <div class="row" style="margin-top: 3rem;margin-bottom:5rem !important;" v-if="isWishListShown">
            <div v-if="isWishListShown" class="m-auto text-center fs-4">
                <p v-if="wishList.books.length == 0" class="text-danger">Wish List is Empty till Now... &nbsp;&nbsp; <button @click="hideCart" type="button" class="btn btn-outline-info">Return To Books</button></p>
            </div>
            <div  v-for="book in wishList.books" :key="book.id" :title="book.author" class="col-sm-12 col-md-6 col-lg-4">
                <div class="card my-3">
                    <img class="card-img-top" :src="book.image" alt="Title">
                    <div class="card-body">
                        <h4 class="card-title">{{book.title}}</h4>
                        <hr>
                        <div class="d-flex justify-content-between m-auto p-2">
                            <div class="left">
                                <h5>{{book.id}}</h5>
                                <h5 :class="book.pages > 50? 'more':'less'">{{book.pages}}</h5>
                                <h5>{{book.category}}</h5>
                            </div>
                            <div class="right">
                                <h5>{{book.author}}</h5>
                                <h5>{{formatter(book.price)}}</h5>
                                <button class="btn btn-danger" @click="removeFromWishList(book)">Remove From Check List</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <!-- End Of Books WishList -->
        
        <div class="row" style="margin-top: 3rem;margin-bottom:5rem !important;">
            <div v-if="isWishListShown" class="d-flex justify-content-center w-75 m-auto">
                <div v-if="wishList.books.length !== 0" class="table-responsive w-100">
                    <hr>
                    <h4 class="text-center">Wish List Books Bill</h4>
                    <hr>
                    <table class="table table-striped table-hover table-info align-middle  text-center">
                        <thead>
                            <caption>Wish List Bill</caption>
                            <tr class="bg-dark text-light">
                                <th>Book Title</th>
                                <th>Author</th>
                                <th>Price</th>
                            </tr>
                        </thead>
                        <tbody class="table-group-divider">
                            <tr class="table-secondary" v-for="book in wishList.books" :key="book.id">
                                <td scope="row">{{book.title}}</td>
                                <td>{{book.author}}</td>
                                <td>{{book.price}}</td>
                            </tr>
                            <tr>
                                <td colspan="3" style="border-bottom: 1px solid black;"></td>
                            </tr>
                        </tbody>
                        <tfoot>
                            <tr>
                                <th>Number Of Books</th>
                                <td colspan="2">{{wishList.books.length}}</td>
                            </tr>
                            <tr>
                                <th>Total Price</th>
                                <td colspan="2">{{calcTotalPrice()}}</td>
                            </tr>
                            <tr>
                                <td colspan="3">
                                    <button class="btn btn-outline-success">Proceed To Payment</button>
                                </td>
                            </tr>
                        </tfoot>
                    </table>
                </div>    
            </div>
        </div>
        <!-- End Of Books Bill -->
    </div>
</template>

<script>
    import {books} from '../data'
    
    export default{
        data() {
            return {
                books,
                isWishListShown: false,
                isBooksShown: true,
                wishList:{
                    books: []
                }
            }
        },
        methods: {
            formatter(input){
                const formatter = new Intl.NumberFormat('ar-SA', {
                style: 'currency',
                currency: 'SAR',
                });

                return formatter.format(input);
            },
            showCart(){
                this.isWishListShown = true;
                this.isBooksShown = false;
            },
            hideCart(){
                this.isWishListShown = false;
                this.isBooksShown = true;
            },
            checkAdded(book){
                return this.wishList.books.some( (currentBook)=>(book === currentBook)  );
            },
            addToWishList(book){
                this.wishList.books.push(book);                    
            },
            removeFromWishList(book){
                let index = this.wishList.books.indexOf(book);
                if( index !== -1 ){
                    this.wishList.books.splice(index,1);
                }
            },
            calcTotalPrice(){
                let total = 0; 
                for(let book in this.wishList.books){
                    total += this.wishList.books[book].price;
                }
                return total;
            }
        },  
    }
</script>

<style scoped>
    .more{
        color: green;
    }
    .less{
        color: orange;
    }
    .none{
        color: red;
    }

    .card:hover{
        cursor: pointer;
        box-shadow: 5px 10px 18px #888888;
    }
</style>