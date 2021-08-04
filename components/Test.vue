<template>
  <div>
    <button @click="testMethod">Click here</button>
  </div>
</template>

<script>
// import Web3 from 'web3'
// import BigNumber from 'bignumber.js'
import Web4 from '@cryptonteam/web4'
import { ERC20 } from '../abi/abis'
// import Web3 from 'web3'

export default {
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
  },
}
</script>
