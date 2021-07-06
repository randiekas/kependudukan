<template>
	<div class="grey lighten-4" style="height:100%">
	<v-container>
		<Head
			title="SIKAD DUDUK"
			subtitle="Sistem Informasi Kependudukan Terintegrasi"
			color="text--black"/>
		<v-row class="mt-2">
			<v-col v-for="(item, index) in apps" :key="index" sm="12" md="3" cols="12">
				<v-card :class="`border--primary ${item.link=='/apps/penduduk'?'primary':''}`" elevation="1" :dark="item.link=='/apps/penduduk'">
					<v-card-title class="pb-0">
						<v-icon left>{{item.ikon}}</v-icon>
						{{item.nama}}
					</v-card-title>
					<v-card-text>
						<div>{{item.deskripsi}}</div>
					</v-card-text>
					<v-card-actions>
						<v-spacer></v-spacer>
						<v-btn :color="item.link=='/apps/penduduk'?'white':'primary'" text :to="item.link" dark>
							Buka
							<v-icon right dark>mdi-launch</v-icon>
						</v-btn>
					</v-card-actions>
				</v-card>
			</v-col>
		</v-row>
		
	</v-container>
	</div>
</template>
<script>
export default {
	layout:'apps',
	props: ['apps'],
	data: () => ({
		
    }),
	methods:{
		
	}
}
</script>
