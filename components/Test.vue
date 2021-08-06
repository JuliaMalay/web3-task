<template>
  <div>
    <button @click="testMethod">Click here</button>
    <button @click="connectWallet">Connect Wallet</button>
    <button @click="guest">Guest</button>
    <button @click="transfer">Transfer</button>
  </div>
</template>

<script>
import Web3 from 'web3'
// import BigNumber from 'bignumber.js'
import Web4 from '@cryptonteam/web4'
import { ERC20 } from '../abi/abis'
// import Web3 from 'web3'

export default {
  data() {
    return {
      userAddress: '',
      recepient: '0xfC9d54AB36ca2bc7E05b6733bd5C8F3837a333C8',
    }
  },
  methods: {
    testMethod() {
      const web4 = new Web4()
      web4.setHDWalletProvider(
        'kitten chase shed local shoe scissors predict lamp news discover crisp violin',
        'https://rinkeby.infura.io/v3/9aa3d95b3bc440fa88ea12eaa4456161'
      )

      // or you can use: web4.setProvider(web3Provider);

      // you can add account by private key
      // web4.privateKeyToAccount("0x348ce564d427a3311b6536bbcff9390d69395b06ed6c486954e971d960fe8709");

      // const BigNumber = require('bignumber.js')
      // let x = new BigNumber(123.4567)
      // let y = BigNumber('123456.7e-3')
      // let z = new BigNumber(x)
      // x.isEqualTo(y) && y.isEqualTo(z) && x.isEqualTo(z)

      const abi = web4.getContractAbstraction(ERC20)

      const test = async () => {
        const tokens = [
          '0x4b107a23361770534bd1839171bbf4b0eb56485c',
          '0xc13da4146d381c7032ca1ed6050024b4e324f4ef',
          '0x8d0c36c8842d1dc9e49fbd31b81623c1902b7819',
          '0xa364f66f40b8117bbdb772c13ca6a3d36fe95b13',
        ]
        for (const token of tokens) {
          const instance = await abi.getInstance(token)
          console.log(instance)
          console.log(await instance.name())
          console.log(await instance.symbol())
          console.log(await instance.decimals())
        }

        // require('web3')
        // if (typeof web3 !== 'undefined') {
        //   web3 = new Web3(
        //     new Web3.providers.HttpProvider('http://localhost:3030')
        //   )
        // }

        // for await (const instance of tokens.map((token) =>
        //   abi.getInstance(token)
        // )) {
        //   console.log(instance)

        // console.log(await instance.name())
        // console.log(await instance.symbol())
        // console.log(await instance.decimals())
        // }

        // адрес токена
        // const instance = await abi.getInstance(
        //   '0x4b107a23361770534bd1839171bbf4b0eb56485c'
        // )
        // const bnBalance = '1500000'
        // const decimal = await instance.decimals()
        // // const bnBalance = new BigNumber(balance).shiftedBy(decimal).toString()
        // console.log(await instance.name())
        // console.log(await instance.symbol())
        // console.log(decimal)

        // await instance.transfer(
        //   '0x4a82895ad5555e517f48eD2a32bBf9Bb4fe08140',
        //   bnBalance
        // )

        // to get encoded abi call use:
        // console.log(
        //   instance.encodeABI(
        //     'transfer',
        //     '0xb346586D70396F8F7936eF8b225501c1EA841e4a',
        //     bnBalance
        //   )
        // )

        // to get event you can use:
        // let events = instance.contract.events.allEvents(
        //   {
        //     fromBlock: '7600000',
        //   },
        //   (error, result) => {
        //     console.log(result)
        //   }
        // )

        return true
      }

      test()
    },
    connectWallet() {
      const BigNumber = require('bignumber.js')
      let web4
      let web3Wallet
      // let userAddress
      let chainId

      const connectWallet = async () => {
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
          console.log(this.userAddress)

          const fetchContractData = async (method, abi, address, params) => {
            try {
              const contract = new web3Wallet.eth.Contract(abi, address)
              return await contract.methods[method].apply(this, params).call()
            } catch (e) {
              console.log(e)
              return ''
            }
          }
          const start = async () => {
            const exampleTokenAddress =
              '0x4b107a23361770534bd1839171bbf4b0eb56485c'
            // const exampleTokenAddress =
            //   '0xc13da4146d381c7032ca1ed6050024b4e324f4ef'
            // const userAddress = '0xBC6ae91F55af580B4C0E8c32D7910d00D3dbe54d'
            const decimals = await fetchContractData(
              'decimals',
              ERC20,
              exampleTokenAddress
            )
            const symbol = await fetchContractData(
              'symbol',
              ERC20,
              exampleTokenAddress
            )
            const name = await fetchContractData(
              'name',
              ERC20,
              exampleTokenAddress
            )
            let balance = await fetchContractData(
              'balanceOf',
              ERC20,
              exampleTokenAddress,
              [this.userAddress]
            )
            balance = new BigNumber(balance).shiftedBy(-decimals).toString()
            console.log(
              `Баланс: ${balance}, Имя: ${name}, Разрядность: ${decimals}, Символ: ${symbol}`
            )
          }
          start()

          const erc20 = web4.getContractAbstraction(ERC20)
          const test = async () => {
            const instance = await erc20.getInstance(
              '0x4b107a23361770534bd1839171bbf4b0eb56485c'
            )

            // console.log(await instance.name())
            // console.log(await instance.symbol())
            // console.log(await instance.decimals())

            // console.log(
            //   await instance.transfer(
            //     '0xfC9d54AB36ca2bc7E05b6733bd5C8F3837a333C8',
            //     15000000
            //   )
            // )
            // console.log(
            //   await instance.approve(
            //     '0xfC9d54AB36ca2bc7E05b6733bd5C8F3837a333C8',
            //     15000000
            //   )
            // )
            const decimals = await fetchContractData(
              'decimals',
              ERC20,
              '0x4b107a23361770534bd1839171bbf4b0eb56485c'
            )
            let allow = await instance.allowance(
              this.userAddress,
              this.recepient
            )
            console.log(
              (allow = new BigNumber(allow).shiftedBy(-decimals).toString())
            )

            return true
          }

          test()

          return true
        } catch (err) {
          console.log(err)
          return false
        }
      }
      connectWallet()
    },
    guest(value) {
      const BigNumber = require('bignumber.js')
      let web3Guest
      // const infura = '9aa3d95b3bc440fa88ea12eaa4456161'
      // const connectNode = () => {
      //   try {
      //     let bscUrl
      //     if (process.env.IS_MAINNET === 'true') {
      //       bscUrl = `wss://mainnet.infura.io/ws/v3/${infura}`
      //     } else {
      //       bscUrl = `wss://rinkeby.infura.io/ws/v3/${infura}`
      //     }
      //     const provider = new Web3.providers.WebsocketProvider(bscUrl)
      //     web3Guest = new Web3(provider)
      //     return true
      //   } catch (e) {
      //     return false
      //   }
      // }
      const fetchContractData = async (method, abi, address, params) => {
        try {
          const contract = new web3Guest.eth.Contract(abi, address)
          return await contract.methods[method].apply(this, params).call()
        } catch (e) {
          console.log(e)
          return ''
        }
      }
      const start = async () => {
        const exampleTokenAddress = '0x4b107a23361770534bd1839171bbf4b0eb56485c'
        // const userAddress = '0xBC6ae91F55af580B4C0E8c32D7910d00D3dbe54d'
        const decimals = await fetchContractData(
          'decimals',
          ERC20,
          exampleTokenAddress
        )
        const symbol = await fetchContractData(
          'symbol',
          ERC20,
          exampleTokenAddress
        )
        let balance = await fetchContractData(
          'balanceOf',
          ERC20,
          exampleTokenAddress,
          [this.userAddress]
        )
        balance = new BigNumber(balance).shiftedBy(-decimals).toString()
        console.log(
          `Баланс: ${balance}, Разрядность: ${decimals}, Символ: ${symbol}`
        )
      }
      // connectNode()
      start()
    },
    transfer() {
      const web4 = new Web4()
      const erc20 = web4.getContractAbstraction(ERC20)
      const test = async () => {
        const instance = await erc20.getInstance(
          '0x4b107a23361770534bd1839171bbf4b0eb56485c'
        )

        console.log(await instance.name())
        console.log(await instance.symbol())
        console.log(await instance.decimals())

        console.log(
          await instance.transfer(
            this.userAddress,
            '0xfC9d54AB36ca2bc7E05b6733bd5C8F3837a333C8',
            10
          )
        )

        // to get encoded abi call use:

        // to get event you can use:
        // let events = instance.contract.events.allEvents({
        //     fromBlock: '7600000'
        // }, (error, result) => {
        //     console.log(result);
        // });

        return true
      }

      test()
    },
  },
}
</script>
