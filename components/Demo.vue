<template>
    <el-container>
        <el-header class="header">
            <img :src="LogoBlanco" alt="BlumSales">
            <h1>BlumSales</h1>
        </el-header>
        <div class="breadcrumb">
            <ol>
                <li>Poscosecha ></li>
                <li>Ingreso de Ramos</li>
            </ol>
        </div>
        <div class="content" v-show="!scan">
            <div class="action">
                <el-button size="small" type="primary" plain>
                    Capturar Inventario
                </el-button>
            </div>
            <div class="filters">
                <div class="filter-title">
                    <el-button
                        size="mini"
                        @click="reset"
                        >
                        Limpiar
                    </el-button>
                    <h2>Filtros</h2>
                    <el-button
                        size="mini"
                        @click="showFilters = !showFilters">
                        <span v-if="showFilters">Ocultar</span>
                        <span v-else>Mostrar</span>
                    </el-button>
                </div>                
                <div class="filter-elements" v-show="showFilters">
                    <div class="filter-element">
                        <label for="date">Fecha</label>
                        <el-date-picker
                            v-model="date"
                            type="date"
                            placeholder="Seleccione la fecha"
                            style="width: 100%">
                        </el-date-picker>
                    </div>
                    <div class="filter-element">
                        <label for="product">Producto</label>
                        <el-select 
                            v-model="product"
                            multiple
                            filterable
                            placeholder="Seleccione Producto"
                            style="width: 100%"
                            >
                            <el-option
                                label="Prueba"
                                value="1"
                            ></el-option>
                        </el-select>
                    </div>
                    <div class="filter-element">
                        <label for="variety">Variedad</label>
                        <el-select 
                            v-model="variety"
                            multiple
                            filterable
                            placeholder="Seleccione Variedad"
                            style="width: 100%"
                            >
                            <el-option
                                label="Prueba"
                                value="1"
                            ></el-option>
                        </el-select>
                    </div>
                    <div class="filter-element">
                        <label for="grade">Grado</label>
                        <el-select 
                            v-model="grade"
                            multiple
                            filterable
                            placeholder="Seleccione Grado"
                            style="width: 100%"
                            >
                            <el-option
                                label="Prueba"
                                value="1"
                            ></el-option>
                        </el-select>
                    </div>
                    <div class="filter-element">
                        <label for="cap">Capuchon</label>
                        <el-select 
                            v-model="cap"
                            multiple
                            filterable
                            placeholder="Seleccione Capuchon"
                            style="width: 100%"
                            >
                            <el-option
                                label="Prueba"
                                value="1"
                            ></el-option>
                        </el-select>
                    </div>
                    <div class="filter-element">
                        <label for="client">Cliente</label>
                        <el-select 
                            v-model="client"
                            multiple
                            filterable
                            placeholder="Seleccione Cliente"
                            style="width: 100%"
                            >
                            <el-option
                                label="Prueba"
                                value="1"
                            ></el-option>
                        </el-select>
                    </div>
                    <div class="filter-button">
                        <el-button
                            type="primary"
                            icon="el-icon-search"
                            @click="filter">
                            Filtrar
                        </el-button>
                    </div>
                </div>
            </div>
            <div class="results" v-show="!showFilters">
                <item-task
                    @showCapture="showCapture"
                    date="2021-11-29"
                    client="Fall River Supply"
                    product="Clavel Don Pedro Rojo 60 CM"
                    cap="Microperforado 30*40*60"
                    :txr="25"
                    :total="60"
                    :pendings="60"
                    />
            </div>
        </div>
        <div class="scan-task" v-if="scan">
            <item-task
                @closeCapture="closeCapture"
                :date="scanData.date"
                :client="scanData.client"
                :product="scanData.product"
                :cap="scanData.cap"
                :txr="scanData.txr"
                :total="scanData.total"
                :pendings="scanData.pendings"
                capture
                />
        </div>
    </el-container>
</template>
<script>
import LogoBlanco from '../assets/images/logoblanco.png'
import Qr from '../assets/images/qr_img.png'
import ItemTask from './ItemTask.vue'
export default {
    components: {
        ItemTask
    },
    data: () => ({
        LogoBlanco,
        Qr,
        showFilters: false,
        scan: false,
        date: '',
        product: '',
        variety: '',
        grade: '',
        client: '',
        cap: '',
        scanData: {
            date: '',
            client: '',
            product: '',
            cap: '',
            txr: 0,
            total: 0,
            pendings: 0
        }
    }),
    methods: {
        reset(){
            this.product = ''
            this.variety = ''
            this.grade = ''
            this.client = ''
            this.cap = ''
        },
        filter(){
            this.showFilters = !this.showFilters
        },
        showCapture(data){
            this.scan = true
            this.scanData = data
        },
        closeCapture(){
            this.scan = false
            this.scanData = {
                date: '',
                client: '',
                product: '',
                cap: '',
                txr: 0,
                total: 0,
                pendings: 0
            }
        }
    },
    created(){
        this.date = new Date()
    }
}
</script>

<style scoped>
.header{
    background-color: #00769B;
    padding: 10px;
}

.header img{
    max-height: 40px;
    vertical-align: middle;
}

h1{
    display: inline-block;
    padding: 0;
    margin: 0;
    font-family: 'Roboto', sans-serif;
    font-weight: bold;
    color: white;
    font-size: 2.3rem;
}

.breadcrumb{
    padding: 10px;
    background-color: rgb(238, 236, 236);
}

.breadcrumb ol {
    padding: 0;
    margin: 0;
}
.breadcrumb ol li{
    padding: 0 10px 0 0;
    margin: 0;
    list-style: none;
    display: inline-block;
    font-size: 1.4rem;
}

.action {
    margin: 10px 0px;
}

.action button{
    width: 100%;
    text-transform: uppercase;
}

.filters {
    background-color: #F5F5F5;
}

.filter-element {
    margin-top: 10px;
}

.filters .filter-title{
    background-color: white;
    border: 1px solid lightgrey;
    padding: 1px 10px;
    text-align: center;
    color: rgb(71, 71, 71);
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.filters .filter-title a{
    font-size: 1.8rem;
}


.filters .filter-element{
    padding: 0px 10px;
}

.filters .filter-element label{
    display: block;
    font-size: 1.2rem;
    font-weight: bold;
    margin-bottom: 5px;
}

.filters .filter-button{
    margin-top: 15px;
}

.filters .filter-button button{
    width: 100%;
    font-size: 1.8rem;
}
</style>