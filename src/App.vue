<template>
	<div class="container text-white p-3 mt-3">
		<h1 class="pt-6">CoinMarket</h1>
		<input
			type="text"
			class="form-control bg-dark text-light border-0 my-4 text-center rounded-0"
			placeholder="Search coin"
			@keyup="searchCoin()"
			v-model="textSearch"
		/>
		<table class="table table-dark">
			<thead>
				<tr>
					<th v-for="title in titles" :key="title">{{ title }}</th>
				</tr>
			</thead>
			<tbody>
				<tr v-for="(coin, i) in filteredCoins" :key="coin.id">
					<td class="text-muted">{{ i + 1 }}</td>
					<td>
						<img :src="coin.image" :alt="coin.name" style="width: 2rem" />
						<span class="ms-3">
							{{ coin.name }}
						</span>
						<span class="text-muted text-uppercase ms-2">
							{{ coin.symbol }}
						</span>
					</td>
					<td>$ {{ coin.current_price.toLocaleString() }}</td>
					<td
						:class="[
							coin.price_change_percentage_24h > 0
								? 'text-success'
								: 'text-danger',
						]"
					>
						{{ coin.price_change_percentage_24h }}
					</td>
					<td>{{ coin.total_volume.toLocaleString() }}</td>
				</tr>
			</tbody>
		</table>
	</div>
</template>

<script>
export default {
	name: 'App',
	data() {
		return {
			titles: ['Position', 'Coin', 'Price', 'Price Change', '24h Volume'],
			coins: [],
			filteredCoins: [],
			textSearch: '',
		};
	},
	async mounted() {
		const res = await fetch(
			'https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false'
		);
		const data = await res.json();
		this.coins = data;
		this.filteredCoins = data;
	},
	methods: {
		searchCoin() {
			this.filteredCoins = this.coins.filter(
				coin =>
					coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
					coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
			);
		},
	},
};
</script>

<style></style>
