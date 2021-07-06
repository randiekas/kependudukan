<template>
	<div class="grey lighten-4 fill-height">
		<v-container>
            <v-card>
                <v-app-bar
                    color="white"
                    elevation="0">
                    <v-btn 
                        icon
                        v-on:click="goBack">
                        <v-icon>mdi-chevron-left</v-icon>
                    </v-btn>

                    <v-toolbar-title>Laporan Penduduk</v-toolbar-title>
                    <v-spacer></v-spacer>
                    <v-btn 
                        v-on:click="handelCetak"
                        small
                        class="primary">
                        <v-icon left>mdi-database-refresh-outline</v-icon>
                        Generate data
                    </v-btn>
                </v-app-bar>
                <v-row class="ml-2 mr-2 pt-0 pb-0 justify-center align-center" dense>
                    <v-col md="1" cols="12">
                        <v-autocomplete
                            label="Tahun"
                            v-model="tahunDipilih"
                            :items="tahun"
                            item-text="nama"
                            item-value="id"/>
                    </v-col>
                    <v-col md="1" cols="12">
                        <v-autocomplete
                            label="Bulan"
                            v-model="bulanDipilih"
                            :items="bulan"
                            item-text="nama"
                            item-value="id"/>
                    </v-col>
                    
                    <v-col md="2" cols="12">
                        <v-autocomplete
                            label="Pilih Kabupaten"
                            v-model="kabupatenDipilih"
                            :items="kabupaten"
                            item-text="nama"
                            item-value="id"/>
                    </v-col>
                    <v-col md="2" cols="12">
                        <v-autocomplete
                            label="Pilih Kecamatan"
                            v-model="kecamatanDipilih"
                            :items="kecamatan"
                            item-text="nama"
                            item-value="id"/>
                    </v-col>
                    <v-col md="2" cols="12">
                        <v-autocomplete
                            label="Pilih Desa"
                            v-model="desaDipilih"
                            :items="desa"
                            item-text="nama"
                            item-value="id"/>
                    </v-col>
                    <v-col md="2" cols="12">
                        <v-btn 
                            small 
                            block
                            v-on:click="handelCetak">
                            <v-icon left small>
                                mdi-printer
                            </v-icon>
                            Cetak / Export PDF
                        </v-btn>
                    </v-col>
                    <v-col md="2" cols="12">
                        <v-btn 
                            small 
                            block
                            v-on:click="handleUpdateData">
                            <v-icon left small>
                                mdi-text-box-search-outline
                            </v-icon>
                            Tampilkan
                        </v-btn>
                    </v-col>
                </v-row>
            </v-card>
            <v-card class="mt-6">
                <v-card-text>
                    <vue-html2pdf
                        :show-layout="true"
                        :float-layout="false"
                        :enable-download="false"
                        :preview-modal="true"
                        filename="print laporan"
                        :paginate-elements-by-height="1100"
                        :pdf-quality="2"
                        pdf-format="a4"
                        pdf-orientation="landscape"
                        pdf-content-width="100%"
                        :manual-pagination="false"
                        ref="html2Pdf">
                        <section slot="pdf-content" style="padding:12px">
                            <p class="text-overline">1. Jumlah Penduduk</p>
                            <small>
                            <table style="width:100%; margin:8px">
                                <thead>
                                    <tr >
                                        <th rowspan="3" class="text-center">NO</th>
                                        <th rowspan="3" class="text-center">DUSUN</th>
                                        <th colspan="6" class="text-center">JUMLAH PENDUDUK&nbsp;</th>
                                        <th colspan="12" class="text-center">PROSES PERUBAHAN PENDUDUK</th>
                                        <th colspan="3" class="text-center">KK</th>
                                    </tr>
                                    <tr >
                                        <th colspan="3" class="text-center">BULAN LALU</th>
                                        <th colspan="3" class="text-center">BULAN INI</th>
                                        <th colspan="3" class="text-center">LAHIR</th>
                                        <th colspan="3" class="text-center">MATI</th>
                                        <th colspan="3" class="text-center">KELUAR</th>
                                        <th colspan="3" class="text-center">DATANG</th>
                                        <th rowspan="2" class="text-center">L</th>
                                        <th rowspan="2" class="text-center">P</th>
                                        <th rowspan="2" class="text-center">JML</th>
                                    </tr>
                                    <tr >
                                        <th>L</th>
                                        <th>P</th>
                                        <th>JUMLAH</th>
                                        <th>L</th>
                                        <th>P</th>
                                        <th>JUMLAH</th>
                                        <th>L&nbsp;</th>
                                        <th>P</th>
                                        <th>JML</th>
                                        <th>L&nbsp;</th>
                                        <th>P</th>
                                        <th>JML</th>
                                        <th>L&nbsp;</th>
                                        <th>P</th>
                                        <th>JML</th>
                                        <th>L</th>
                                        <th>P</th>
                                        <th>JML</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    
                                    <tr >
                                        <td>1</td>
                                        <td>DUSUN 1</td>
                                        <td>10</td>
                                        <td>8</td>
                                        <td>18</td>
                                        <td>12</td>
                                        <td>12</td>
                                        <td>24</td>
                                        <td>2</td>
                                        <td>2</td>
                                        <td>4</td>
                                        <td>&nbsp;</td>
                                        <td>&nbsp;</td>
                                        <td>0</td>
                                        <td>&nbsp;</td>
                                        <td>&nbsp;</td>
                                        <td>0</td>
                                        <td>&nbsp;</td>
                                        <td>2</td>
                                        <td>2</td>
                                        <td>8</td>
                                        <td>8</td>
                                        <td>16</td>
                                    </tr>
                                </tbody>
                                
                            </table>
                            </small>
                            <p class="text-overline">2. Penduduk Berdasarkan Agama</p>
                            <p class="text-overline">3. Penduduk Berdasarkan Status Perkawinan</p>
                            <p class="text-overline">4. Penduduk Berdasarkan Pendidikan</p>
                            <p class="text-overline">5. Penduduk Berdasarkan Pekerjaan</p>
                        </section>
                    </vue-html2pdf>
                </v-card-text>
            </v-card>    
		</v-container>
	</div>
</template>
<script>
import VueHtml2pdf from 'vue-html2pdf'
export default {
    layout:'apps',
    components: {
        VueHtml2pdf
    },
	props: [],
    async asyncData({ $api }) {
        let grup        = [
                            'Jumlah Penduduk',
                            'Berdasarkan Agama',
                            'Berdasarkan Status Perkawinan',
                            'Berdasarkan Pendidikan',
                            'Berdasarkan Pekerjaan',
                        ]
		var provinsiDipilih = 1
        let kabupaten   = (await $api.$get(`/v1/api/query/master_kabupaten?where=id_provinsi=${provinsiDipilih}`)).data
        let kecamatan   = (await $api.$get(`/v1/api/query/master_kecamatan?where=id_kabupaten=${kabupaten[0].id}`)).data
        let desa        = (await $api.$get(`/v1/api/query/master_desa?where=id_kecamatan=${kecamatan[0].id}`)).data
        
		return {
            grup,
            grupDipilih: 0,
            tahun: [2021],
            tahunDipilih: 2021,
            bulan: ['Januari', 'Februari', 'Maret', 'April', 'Mei', 'Juni', 'Juli', 'Agustus', 'September', 'November', 'Oktober', 'Desember'],
            bulanDipilih: 'Juli',
            provinsiDipilih: 1,
            kabupaten,
            kabupatenDipilih: kabupaten[0].id,
            kecamatan,
            kecamatanDipilih: kecamatan[0].id,
            desa,
            desaDipilih: desa[0].id,
		}
	},
    watch: {
        provinsiDipilih: async function (val) {
            this.kabupaten                  = (await this.$api.$get(`/v1/api/query/master_kabupaten?where=id_provinsi=${val}`)).data
            this.kecamatan                  = []
            this.desa                       = []
        },
        kabupatenDipilih: async function (val) {
            this.kecamatan                  = (await this.$api.$get(`/v1/api/query/master_kecamatan?where=id_kabupaten=${val}`)).data
            this.desa                       = []
        },
        kecamatanDipilih: async function (val) {
            this.desa                       = (await this.$api.$get(`/v1/api/query/master_desa?where=id_kecamatan=${val}`)).data
        },
        desaDipilih: async function (val) {
            this.crud.apiData               = `${this.crud.api}?where=id_desa=${val}`
            this.crud.headers[0].default    = val
        },
    },
    beforeMount: function(){
        this.crud.apiData               = `${this.crud.api}?where=id_desa=${this.desaDipilih}`
        this.crud.headers[0].default    = this.desaDipilih
    },
	data(){
        return {
            crud: {
                nested: true,
                title: "Laporan bulanan",
                subtitle: "Kelola data dusun",
                api: `/v1/api/query/master_dusun`,
                apiData: `/v1/api/query/master_dusun`,
                apiTambah: `/v1/api/tambah/master_dusun`,
                apiUbah: `/v1/api/ubah/master_dusun`,
                apiHapus: `/v1/api/hapus/master_dusun`,

                headers: [                    
                    {
                        text: 'Dusun',
                        value: 'dusun',
                    }, 
                    {
                        text: 'Bulan Lalu', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                    {
                        text: 'Bulan Ini', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                    {
                        text: 'Lahir', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                    {
                        text: 'Mati', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                    {
                        text: 'Keluar', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                    {
                        text: 'Datang', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                    {
                        text: 'KK', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                ],
            },
            data:[]
        }
    },
	methods:{
        goBack(){
            history.back()
        },
        handelCetak(){
			this.$refs.html2Pdf.generatePdf()
		},
        handleUpdateData: function(){

        },
		handelFilterGrup: function(index){
            this.grupDipilih    = index
            const headers       = [
                [
                    {
                        text: 'Dusun',
                        value: 'dusun',
                    }, 
                    {
                        text: 'Bulan Lalu', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                    {
                        text: 'Bulan Ini', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                    {
                        text: 'Lahir', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                    {
                        text: 'Mati', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                    {
                        text: 'Keluar', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                    {
                        text: 'Datang', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                    {
                        text: 'KK', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                ],
                [
                    {
                        text: 'Dusun',
                        value: 'dusun',
                    }, 
                    {
                        text: 'Islam', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                    {
                        text: 'Kristen', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                    {
                        text: 'Khatolik', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                    {
                        text: 'Hindu', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                    {
                        text: 'Budha', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    }, 
                    {
                        text: 'Jumlah', 
                        value: 'jumlah', 
                        align: 'center',
                    }, 
                ],
                [
                    {
                        text: 'Dusun',
                        value: 'dusun',
                    }, 
                    {
                        text: 'Belum Kawin', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'Kawin', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'Janda', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'Duda', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'Jumlah', 
                        value: 'jumlah',
                        align: 'center',
                    }
                ],
                [
                    {
                        text: 'Dusun',
                        value: 'dusun',
                    }, 
                    {
                        text: 'Belum', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'SD', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'SMP', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'SMA', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'D1', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'D2', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'D3', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'S1/D4', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'S2', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'S3', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'Jumlah', 
                        align: 'center',
                        value: 'jumlah',
                    }
                ],
                [
                    {
                        text: 'Dusun',
                        value: 'dusun',
                    }, 
                    {
                        text: 'Belum Bekerja', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'Petani', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'Nelayan', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'Wiraswasta', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'PNS', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'Honorer', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'Karyawan Swasta', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'TNI/Abri', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'Polisi', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'Lainnya', 
                        align: 'center',
                        children:[
                            {
                                text: 'L',
                                value: 'l',
                            },
                            {
                                text: 'P',
                                value: 'p',
                            },
                            {
                                text: 'jml',
                                value: 'jml',
                            },
                        ]
                    },
                    {
                        text: 'Jumlah',
                        value: 'jumlah',
                        align: 'center',
                    }
                ]
            ]
            this.crud.headers   =   headers[index]
        }
	}
}
</script>
<style>
table, th, td
{
    border-collapse:collapse;
    border: 1px solid black;
    text-align:center;
}
</style>