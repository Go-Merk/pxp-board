<script>
	import { daily_volume } from './assets/js/store.js';
	import { onMount } from 'svelte';
	import {fade} from 'svelte/transition';
	import {Route} from 'tinro';

	import Savings from './routes/Savings.svelte'
	import Staking from './routes/Staking.svelte'
	import Donate from './routes/Donate.svelte'

	import Alert from './components/Alert.svelte'
	import Banner from './components/Banner.svelte'
	import BraveButton from './components/BraveButton.svelte'
	import Chart from './components/Chart.svelte'
	import Crypto from './components/Crypto.svelte'
	import Exchanges from './components/Exchanges.svelte'
	import Fab from './components/Fab.svelte'
	import Footer from './components/Footer.svelte'
	import NavigationBar from './components/NavigationBar.svelte'
	import NavigationRail from './components/NavigationRail.svelte'
	import OnStake from './components/OnStake.svelte'
	import OnDonate from './components/OnDonate.svelte'
	import ClipboardButton from './components/ClipboardButton.svelte'
	import TokenSupply from './components/TokenSupply.svelte'
	import TooltipButton from './components/TooltipButton.svelte'
	import TopAppBar from './components/TopAppBar.svelte'
	import TotalVolume from './components/TotalVolume.svelte'

	let alert;
	let alert_msg;
	let quest = Math.floor((Math.random() * 10));
	let theme = localStorage.theme;

	(async () => {
		$daily_volume = false
		let gecko = await fetch('https://api.coingecko.com/api/v3/coins/pointpay/tickers?&depth=false')
		let solidbit = await fetch('https://b2t-api-cmc-solidbit.flexprotect.org/marketdata/cmc/v1/ticker')

		const ex1 = await gecko.json()
		const ex2 = await solidbit.json()

		for (const exchange of ex1.tickers) {
  		$daily_volume += exchange.converted_volume.usd;
		}

		$daily_volume += parseFloat(ex2.PXP_USDT.quote_volume)
		console.log($daily_volume)
	})()

	function changeTheme() {
		document.body.className = ''
		document.body.classList.add(`theme-${theme}`)
	}

	function changeIcon(event) {
		theme = event.detail
		changeTheme()
		localStorage.setItem('theme', theme);
	}

	function showAlert(event) {
    alert_msg = event.detail;
		clearTimeout(alert)
		alert = setTimeout(() => {
			alert_msg = "";
		}, 7500)
  }

	window.matchMedia('(prefers-color-scheme: dark)')
		.addEventListener('change', event => {
    	theme = event.matches ? "dark" : "light";
			changeTheme()
		});

	onMount(() => {
		if (theme) {
			theme = localStorage.theme
		} else {
			theme = (() => {
				if (window.matchMedia('(prefers-color-scheme: dark)').matches) {
					return "dark"
				} else {
					return "light"
				}
			})()
		}
		changeTheme()
	});
</script>

<TopAppBar mode="{theme}" on:message={changeIcon}/>

<NavigationRail/>

<main>
	<div class="live-area">
		<Route path="/">
			<section in:fade="{{delay: 100, duration: 250 }}" out:fade="{{ duration: 100 }}">
				<TotalVolume mobile/>
				<Chart/>
				<TokenSupply/>
				<Exchanges/>
				<Banner quest="{quest}"/>
			</section>
		</Route>
		<Route path="/savings">
			<Savings>
				<Crypto name="PointPay" ticker="PXP" color="positive" value="up to 10.0%"/>
				<Crypto name="Bitcoin" ticker="BTC" color="positive" value="1.2%"/>
				<Crypto name="Ethereum" ticker="ETH" color="positive" value="1.0%"/>
				<Crypto name="Tether" ticker="USDT" color="positive" value="1.1%"/>
				<Crypto name="USD Coin" ticker="USDC" color="positive" value="1.1%"/>
				<Crypto name="Bitcoin Cash" ticker="BCH" color="positive" value="1.0%"/>
				<Crypto name="Litecoin" ticker="LTC" color="positive" value="1.0%"/>
				<Crypto name="Ripple" ticker="XRP" color="positive" value="1.0%"/>
				<Crypto name="ChainLink" ticker="LINK" color="positive" value="1.0%"/>
				<Crypto name="Tron" ticker="TRX" color="positive" value="1.0%"/>
				<Crypto name="Dai" ticker="DAI" color="positive" value="1.1%"/>
				<Crypto name="Binance Coin" ticker="BNB" color="positive" value="1.0%"/>
				<Crypto name="Tether Gold" ticker="XAUT" color="positive" value="0.3%"/>
				<Crypto name="Tetha Network" ticker="TETHA" color="positive" value="0.3%"/>
				<Crypto name="Uniswap" ticker="UNI" color="positive" value="0.1%"/>
				<Crypto name="Stellar" ticker="XML" color="positive" value="0.1%"/>
				<Crypto name="SushiSwap" ticker="SUSHI" color="positive" value="0.1%"/>
				<Crypto name="Solana" ticker="SOL" color="positive" value="0.1%"/>
				<Crypto name="Polygon" ticker="MATIC" color="positive" value="0.1%"/>
				<Crypto name="NEAR Protocol" ticker="NEAR" color="positive" value="0.1%"/>
				<Crypto name="Cosmos" ticker="ATOM" color="positive" value="0.1%"/>
				<Crypto name="Polkadot" ticker="DOT" color="positive" value="0.1%"/>
				<Crypto name="PancakeSwap" ticker="CAKE" color="positive" value="0.1%"/>
				<Crypto name="Ethereum Classic" ticker="ETC" color="positive" value="0.1%"/>
				<Crypto name="Dash" ticker="DASH" color="positive" value="0.1%"/>
				<Crypto name="Avalanche" ticker="AVAX" color="positive" value="0.1%"/>
			</Savings>
		</Route>
		<Route path="/staking">
			<Staking>
				<OnStake quantity="0 ~ 100 PXP" on_stake03="2.0%" on_stake06="4.5%" on_stake12="7.5%"/>
				<OnStake quantity="101 ~ 15,000 PXP" on_stake03="3.0%" on_stake06="5.5%" on_stake12="8.5%"/>
				<OnStake quantity="15,001 ~ 35,000 PXP" on_stake03="5.5%" on_stake06="7.0%" on_stake12="10.0%"/>
				<OnStake quantity="35,001 ~ 70,000 PXP" on_stake03="6.0%" on_stake06="8.0%" on_stake12="11.5%"/>
				<OnStake quantity="70,001 ~ 100,000 PXP" on_stake03="7.0%" on_stake06="9.0%" on_stake12="12.5%"/>
				<OnStake quantity="100,001 ~ 300,000 PXP" on_stake03="8.0%" on_stake06="10.0%" on_stake12="15.0%"/>
				<OnStake quantity="300,001 ~ 1,000,000 PXP" on_stake03="9.0%" on_stake06="12.5%" on_stake12="17.5%"/>
				<OnStake quantity="1,000,001+ PXP" on_stake03="10.0%" on_stake06="15.0%" on_stake12="20.0%"/>
			</Staking>
		</Route> 
		<Route path="/donate">
			<Donate>
				<OnDonate
					name="Pix"
					description="Ei! Você é brasileiro? Eu também! Para apoiar o projeto basta usar o Pix para fazer uma doação."
					image="pix.svg"
					color="#32BCAD">
					<ClipboardButton
						label="Chave Pix"
						value="katriel.crypto@gmail.com"
						msg="Chave Pix copiada! Agora, dirija-se ao app do seu banco para fazer a doação."
						on:alert={showAlert}/>
					<TooltipButton icon="qrcode">
						<img src="/qr/pix-qr.svg" alt="QR Code - Donate with Pix"/>
					</TooltipButton>
				</OnDonate>
				<OnDonate
					name="PointPay Me"
					description="Contribute to the project using PointPay Me, conveniently send crypto by email."
					image="pointpay-me.svg"
					color="var(--primary)">
					<ClipboardButton
						label="Email"
						value="katriel.crypto@gmail.com"
						msg="Email copied! Now you can go to PointPay App/Website to make the donation."
						on:alert={showAlert}/>
				</OnDonate>
				<OnDonate
					name="Binance Pay"
					description="Donate 70+ cryptocurrencies at ZERO fees using Binance Pay! Fast and easy!"
					image="binance.svg"
					color="#F3BA2F">
					<ClipboardButton
						label="Email"
						value="katriel.crypto@gmail.com"
						msg="Email copied! Now you can go to Binance App/Website to make the donation."
						on:alert={showAlert}/>
					<div>
						<TooltipButton icon="qrcode">
							<img src="/qr/binance-qr.svg" alt="QR Code - Donate with Binance Pay"/>
						</TooltipButton>
					</div>
				</OnDonate>
				<OnDonate
					name="Brave Rewards"
					image="brave.svg"
					color="#FB542B"
					description="PXP Board is a verified Brave Creator, tipping BAT is supported. Not using Brave? Give it a try!">
					<BraveButton/>
				</OnDonate>
			</Donate>
		</Route> 
	</div>
</main>

<Fab/>
<Footer/>
<NavigationBar/>

{#if alert_msg}
	<Alert msg="{alert_msg}" />
{/if}


<style lang="stylus">
main
	margin-bottom 32px
	padding 0 16px
	flex 1

	@media (min-width 600px)
		padding 0 32px

	@media (min-width 840px)
		padding 0 64px

	@media (min-width 1080px)
		padding 0 96px

.live-area
	display grid
	grid-template-rows 1fr
	grid-template-columns 1fr

	& > :global(*)
		grid-row 1
		grid-column 1
</style>