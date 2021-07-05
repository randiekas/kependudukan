<template>
	<div class="grey lighten-4" style="height:100%">
		<v-container>
		<Crud
			:crud="crud">
            <template v-slot:action>
                <v-btn 
                    small 
                    color="primary"
                    class="d-none d-sm-flex">
                    <v-icon left>mdi-information-outline</v-icon>
                    data diperbaharui setiap jam 12 malam
                </v-btn>
            </template>
            <template v-slot:filter="{handleUpdateData}">
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
                            label="Pilih Provinsi"
                            v-model="provinsiDipilih"
                            :items="provinsi"
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
                            v-on:click="handleUpdateData">
                            <v-icon left small>
                                mdi-text-box-search-outline
                            </v-icon>
                            Tampilkan
                        </v-btn>
                    </v-col>
                </v-row>
                <div>
                    <v-chip 
                        v-for="(item, index) in grup" :key="index"
                        class="ma-2"
                        :color="index===grupDipilih?'primary':'default'"
                        v-on:click="handelFilterGrup(index)">
                            {{item}}
                    </v-chip>
                </div>
            </template>
        </Crud>
		</v-container>
	</div>
</template>
<script>
export default {
	layout:'apps',
	props: [],
    async asyncData({ $api }) {
        let grup        = [
                            'Jumlah Penduduk',
                            'Berdasarkan Agama',
                            'Berdasarkan Status Perkawinan',
                            'Berdasarkan Pendidikan',
                            'Berdasarkan Pekerjaan',
                        ]
		let provinsi    = (await $api.$get(`/v1/api/data/master_provinsi`)).data
        let kabupaten   = (await $api.$get(`/v1/api/query/master_kabupaten?where=id_provinsi=${provinsi[0].id}`)).data
        let kecamatan   = (await $api.$get(`/v1/api/query/master_kecamatan?where=id_kabupaten=${kabupaten[0].id}`)).data
        let desa        = (await $api.$get(`/v1/api/query/master_desa?where=id_kecamatan=${kecamatan[0].id}`)).data
        
		return {
            grup,
            grupDipilih: 0,
            tahun: [2021],
            tahunDipilih: 2021,
            bulan: ['Januari', 'Februari', 'Maret', 'April', 'Mei', 'Juni', 'Juli', 'Agustus', 'September', 'November', 'Oktober', 'Desember'],
            bulanDipilih: 'Juli',
			provinsi,
            provinsiDipilih: provinsi[0].id,
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
