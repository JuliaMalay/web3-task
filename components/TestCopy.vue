<template>
  <div>
    <p>Token:</p>
    <input v-model="token" type="text" />
    <p>Recepient:</p>
    <input v-model="recepient" type="text" />
    <p>Amount:</p>
    <input v-model="amount" type="text" />

    <button @click="connectWallet">Connect Wallet</button>
    <button @click="getToken">Get token</button>
    <button @click="transfer">Transfer</button>
    <button @click="approve">Approve</button>
    <button @click="allowance">Allowance</button>
    <button @click="history">History</button>
  </div>
</template>

<script>
import Web3 from 'web3'
import Web4 from '@cryptonteam/web4'
import { ERC20 } from '../abi/abis'
const BigNumber = require('bignumber.js')
let web4
let web3Wallet
let chainId
let decimals
let instance
let amountBig

export default {
  data() {
    return {
      userAddress: '',
      recepient: '0xfC9d54AB36ca2bc7E05b6733bd5C8F3837a333C8',
      tokesAdress: [
        '0x4b107a23361770534bd1839171bbf4b0eb56485c',
        '0xc13da4146d381c7032ca1ed6050024b4e324f4ef',
        '0x8d0c36c8842d1dc9e49fbd31b81623c1902b7819',
        '0xa364f66f40b8117bbdb772c13ca6a3d36fe95b13',
      ],
      token: '0x4b107a23361770534bd1839171bbf4b0eb56485c',
      amount: 15,
    }
  },
  methods: {
    async fetchContractData(method, abi, address, params) {
      try {
        const contract = new web3Wallet.eth.Contract(abi, address)
        return await contract.methods[method].apply(this, params).call()
      } catch (e) {
        console.log(e)
        return ''
      }
    },
    async getToken() {
      decimals = await this.fetchContractData('decimals', ERC20, this.token)
      const symbol = await this.fetchContractData('symbol', ERC20, this.token)
      const name = await this.fetchContractData('name', ERC20, this.token)
      let balance = await this.fetchContractData(
        'balanceOf',
        ERC20,
        this.token,
        [this.userAddress]
      )
      balance = new BigNumber(balance).shiftedBy(-decimals).toString()
      console.log(
        `Баланс: ${balance}, Имя: ${name}, Разрядность: ${decimals}, Символ: ${symbol}`
      )
    },
    async connectWallet() {
      try {
        const { ethereum } = window // ethereum - metamask
        if (!ethereum) {
          console.log('metamask is not install')
          return false
        }
        web3Wallet = new Web3(ethereum) // init web3
        if ((await web3Wallet.eth.getCoinbase()) === null) {
          // проверяем подключен ли metamask
          await ethereum.enable() // подключить metamask
        }
        this.userAddress = await web3Wallet.eth.getCoinbase() // получить адрес пользователя
        chainId = await web3Wallet.eth.net.getId() // запись сети
        if (+chainId !== 4) {
          console.log('current project work on rinkeby network')
          return false
        }

        web4 = new Web4()
        await web4.setProvider(ethereum, this.userAddress)

        const erc20 = web4.getContractAbstraction(ERC20)
        instance = await erc20.getInstance(this.token)
        console.log('Connected!')

        return true
      } catch (err) {
        console.log(err)
        return false
      }
    },
    async transfer() {
      amountBig = new BigNumber(this.amount).shiftedBy(+decimals).toString()
      console.log(amountBig)
      console.log(await instance.transfer(this.recepient, amountBig))
    },
    async approve() {
      amountBig = new BigNumber(this.amount).shiftedBy(+decimals).toString()
      console.log(amountBig)
      console.log(await instance.approve(this.recepient, amountBig))
    },
    async allowance() {
      let allow = await instance.allowance(this.userAddress, this.recepient)
      console.log(
        (allow = new BigNumber(allow).shiftedBy(-decimals).toString())
      )
    },
    history() {
      const inst = new web3Wallet.eth.Contract(ERC20, this.token)

      // inst.events.allEvents(
      //   {
      //     fromBlock: 0,
      //     filter: {
      //       from: this.userAddress,
      //       to: this.userAddress,
      //       owner: this.userAddress,
      //     },
      //   },
      //   (error, result) => {
      //     console.log('All:')
      //     console.log(error, result)
      //   }
      // )

      // inst.events.Approval(
      //   {
      //     fromBlock: 0,
      //     filter: {
      //       owner: this.userAddress,
      //     },
      //   },
      //   (error, result) => {
      //     console.log('Approval From:')
      //     console.log(error, result)
      //   }
      // )
      inst.events.Transfer(
        {
          fromBlock: 0,
          filter: {
            from: this.userAddress,
          },
        },
        (error, result) => {
          console.log('Transfer From:')
          console.log(error, result)
        }
      )

      // inst.events.Transfer(
      //   {
      //     fromBlock: 0,
      //     filter: {
      //       to: this.userAddress,
      //     },
      //   },
      //   (error, result) => {
      //     console.log('Transfer To:')
      //     console.log(error, result)
      //   }
      // )
    },
  },
}
</script>
