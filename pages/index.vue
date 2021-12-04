<template>
  <v-container id="main-container" fluid>
    <div class="navbar">
      <v-btn class="mx-3" href="https://discord.gg/rkAQw2NQSc" target="_blank" icon rounded>
        <v-icon size="xxx-large">mdi-discord</v-icon>
      </v-btn>
      <v-btn class="mx-3" href="https://www.instagram.com/sofianepamart" target="_blank" icon rounded>
        <v-icon size="xxx-large">mdi-instagram</v-icon>
      </v-btn>
      <v-btn class="mx-3" href="https://www.youtube.com/c/SofianePamart" target="_blank" icon rounded>
        <v-icon size="xxx-large">mdi-youtube</v-icon>
      </v-btn>
      <v-spacer></v-spacer>
      <div>
        <v-btn text height="60" href="https://www.piano-king.com" target="_blank">
          <v-img contain width="100" src="img/piano-king-logo.png" />
        </v-btn>
        <RulesModal />
      </div>
    </div>
    <div class="main-container">
        <h1>Welcome to the Lion NFT room</h1>
        <h2>The party crasher is also a king</h2>
        <button @click="metamaskButtonClick" id="metamask-btn">Connect your Metamask Wallet</button>
      </div>
    <v-img :aria-disabled="loading" contain class="lion-king-image" position="bottom center" lazy-src="img/lion-king-lazy.png" src="img/lion-king.png"></v-img>
  </v-container>
</template>

<script>
import detectEthereumProvider from '@metamask/detect-provider'

export default {
	name: 'index-page',

	data(){
		return{
			currentAccount: '',
			provider: null,
			loading: false
		}
	},

	async mounted() {
		this.provider = await detectEthereumProvider()

		if (this.provider) {
			if (this.provider !== window.ethereum) {
				alert('Do you have multiple wallets installed?')
			}

			this.provider.on('accountsChanged', () => this.handleAccountsChanged)

		} else {
			const ua = navigator.userAgent
			const isOS = /iPhone|iPad|iPod/i.test(ua)
			const isAndroid = /Android/i.test(ua)
			const isDesktop = !(/Android|webOS|iPhone|iPad|iPod/i.test(ua))

			if (isOS) {
				alert('Please use the dApp browser inside Metamask app !')
				window.location.href = 'https://metamask.app.link/skAH3BaF99'
			}

			if (isAndroid) {
				alert('Please use the dApp browser inside Metamask app !')
				window.location.href = 'https://metamask.app.link/bxwkE8oF99'
			}

			if (isDesktop) {
				alert('Please install MetaMask !')
				window.open('https://metamask.io/download.html', '_blank')
			}
		}
	},

	methods: {

		metamaskButtonClick(e){
			if(this.loading)
				return

			this.animateButton(e)
			this.metamaskConnect()
		},

		animateButton(e) {
			e.preventDefault()
			//reset animation
			e.target.classList.remove('animate')

			e.target.classList.add('animate')
			setTimeout(function () {
				e.target.classList.remove('animate')
			}, 700)
		},

		async metamaskConnect() {
			try{
				this.loading = true
				await this.provider.request({ method: 'eth_requestAccounts' })
				this.isConnected = true
			} catch (e){
				alert(e.message)
			} finally {
				this.loading = false
			}
		},

		handleAccountsChanged(accounts) {
			if (accounts.length === 0) {
				// MetaMask is locked or the user has not connected any accounts
				console.log('Please connect to MetaMask.')
			} else if (accounts[0] !== this.currentAccount) {
				this.currentAccount = accounts[0]
			}
		}
	}
}
</script>

<style>
.navbar{
  position: absolute;
  display: flex;
  left: 6vw;
  margin: 1vh 0;
  right: 6vw;
  z-index: 1;
}
</style>
