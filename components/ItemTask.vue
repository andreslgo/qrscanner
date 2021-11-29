<template>
    <div class="item">
        <div class="row">
            <div class="item-element">
                <label for="">Fecha</label>
                <span>{{ date }}</span>
            </div>
            <div class="item-element">
                <label for="">Cliente</label>
                <span>{{ client }}</span>
            </div>
        </div>
        <div class="row">
            <div class="item-element">
                <label for="">Producto</label>
                <span>{{ product }}</span>
            </div>
        </div>
        <div class="row">
            <div class="item-element">
                <label for="">Capuchon</label>
                <span>{{ cap }}</span>
            </div>
            <div class="item-element">
                <label for="">TxR</label>
                <span>{{ txr }}</span>
            </div>
        </div>
        <div class="row">
            <div class="item-element">
                <label for="">Solicitados</label>
                <span>{{ total }}</span>
            </div>
            <div class="item-element">
                <label for="">Pendientes</label>
                <span>{{ pendings }}</span>
            </div>
            <div class="item-element" v-if="capture">
                <label for="">Actuales</label>
                <span>{{ currently }}</span>
            </div>
            <div class="item-element" v-if="!capture">
                <el-button size="small" type="warning" @click="goToCapture">
                    <img :src="Qr" class="qr">
                    Capturar
                </el-button>
            </div>
        </div>
        <div v-if="capture">
            <div class="row">
                <qrcode-stream @decode="decode">

                </qrcode-stream>
                <!--<form @submit.prevent="captureBarcode">
                    <input type="text" ref="barcodeCapture" v-model="barcode" style="font-size: 2.4rem; width: 100%;">
                </form>-->

            </div>
            <div class="row" style="justify-content: center; margin-top: 30px;">
                <el-button
                    type="info"
                    @click="closeCapture"
                    >
                    Terminar Captura
                </el-button>
            </div>
        </div>
            
    </div>
</template>
<script>
import Qr from '../assets/images/qr_img.png'
import { QrcodeStream } from 'vue-qrcode-reader'
export default {
    components: {
        QrcodeStream
    },
    props: {
        date: {
            required: true
        },
        client: {
            type: String,
            required: true
        },
        product: {
            type: String,
            required: true
        },
        cap: {
            type: String,
            required: true
        },
        txr: {
            type: Number,
            required: true
        },
        total: {
            type: Number,
            required: true
        },
        pendings: {
            type: Number,
            required: true
        },
        capture: {
            type: Boolean,
            default: false
        }
    },
    data: () => ({
        Qr,
        barcode: '',
        currently: 0
    }),
    methods: {
        decode(decodeString){
            alert(decodeString)
        },
        goToCapture(){
            let data = {
                date: this.date,
                client: this.client,
                product: this.product,
                cap: this.cap,
                txr: this.txr,
                total: this.total,
                pendings: this.pendings
            }
            this.$emit('showCapture', data)
        },
        closeCapture(){
            this.$emit('closeCapture')
        },
        captureBarcode(){
            this.barcode = ''
            this.currently++
            this.$refs.barcodeCapture.focus()
        }
    }
}
</script>

<style scoped>
.row {
    padding: 5px 10px;
    display: flex;
    justify-content: space-between;
}

.item{
    margin: 2px 0px;
    border: 1px solid lightgrey;
}

.item-element label{
    display: block;
    font-weight: bold;
}

.item-element span{
    font-size: 1.6rem;
}
</style>