<template>
    <button id="checkout-button" @click="handleClick">Checkout</button>    
</template>

<script>
import {loadStripe} from '@stripe/stripe-js'

export default {
    data() {
        return {
            token: null
        }
    },
    methods: {
            handleClick() {
                loadStripe("pk_test_42xi7lAoc2wOsFfmZOq7etPS005IDp2SJJ").then(stripe => {
                    console.log('stripe: ', stripe)
                    uni.request({
                        url: 'http://47.95.239.228:8091/api/pay/stripe/?track=12',
                        method: 'GET',
                        header: {
                            Authorization: this.token,
                            ContentType: "application/json"
                        }
                    }).then(([err, res]) => {
                        console.log(res, "\nsession: ",session)
                        const session = res.data
                            
                            stripe.redirectToCheckout({
                                sessionId: session.id
                            }).then(result => {
                                if(result.error) {
                                    uni.showModal({
                                        title: "Error",
                                        content: 'Browser or network error occurred.',
                                        showCancel: false
                                    })
                                }
                            })
                    })
            })
        }
    },
    onLoad: function (option) {
        console.log('isLogin: ', option.isLogin)
        this.token = option.isLogin
    }
}
</script>