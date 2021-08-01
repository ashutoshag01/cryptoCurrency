import Web3 from 'web3'
const Tx = require(`ethereumjs-tx`)

const companyAddress = `Add Your Own/Website/Company Address`
const pk = `Add Your Own/Website/Company Private Key`

const infuraUrl = `Create A Url from Infura`

const abiArray = JSON.parse(
  `get the AbiArray for the contract deployed`,
  `utf-8`
)
const contractAddress = `Address of the contract deployed`

const currGasPrice = 2 * 1e9
const currGasLimit = 210000

export function sendTransactionToCompanyViaMetaMask(
  numberOfCoins,
  handleResponse
) {
  // for connecting with MetaMask
  const web3 = new Web3(Web3.givenProvider || `ws://localhost:8545`)

  web3.eth.requestAccounts().then((accounts) => {
    const sender = accounts[0]
    const amount = web3.utils.toHex(numberOfCoins * 10 ** 18)
    const contract = new web3.eth.Contract(abiArray, contractAddress, {
      from: sender
    })
    web3.eth.sendTransaction(
      {
        to: contractAddress,
        from: sender,
        value: `0x0`,
        data: contract.methods.transfer(companyAddress, amount).encodeABI(),
        gasPrice: web3.utils.toHex(currGasPrice),
        gasLimit: web3.utils.toHex(currGasLimit)
      },
      handleResponse
    )
  })
}

functoin handleResponse(resp, err) {
    // do something    
}

export function sendCoinsToUser(
  userAddress,
  numberOfCoins,
  handleResponse
) {
  const web3 = new Web3(new Web3.providers.HttpProvider(infuraUrl))
  const myAddress = companyAddress
  const toAddress = userAddress

  const amount = web3.utils.toHex(numberOfCoins)

  // get transaction count to be used as nonce
  web3.eth.getTransactionCount(myAddress).then(function(transactionsCount) {
    const count = transactionsCount
    
    const privateKey = new Buffer.from(pk, `hex`)

    const contract = new web3.eth.Contract(abiArray, contractAddress, {
      from: myAddress
    })

    const rawTransaction = {
      from: myAddress,
      gasPrice: web3.utils.toHex(currGasPrice),
      gasLimit: web3.utils.toHex(currGasLimit),
      to: contractAddress,
      value: `0x0`,
      data: contract.methods.transfer(toAddress, amount).encodeABI(),
      nonce: web3.utils.toHex(count)
    }

    const transaction = new Tx(rawTransaction)
    transaction.sign(privateKey)
    const hexCode = `0x`
    const transcationData = hexCode.concat(
      transaction.serialize().toString(`hex`)
    )

    web3.eth.sendSignedTransaction(transcationData, handleResponse)
  })
}

export function getBalanceFromMetaMask() {
  const web3 = new Web3(Web3.givenProvider || `ws://localhost:8545`)
  web3.eth.requestAccounts().then((accounts) => {
    // console.log('Account is ' + accounts[0])
    web3.eth.getBalance(accounts[0]).then(() => {
      // console.log('Balance is ' + balance)
    })
  })
}

export function getBalanceFromAddress(address) {
  const web3 = new Web3(new Web3.providers.HttpProvider(infuraUrl))
  const contract = new web3.eth.Contract(abiArray, contractAddress, {
    from: address
  })

  return contract.methods.balanceOf(address).call()
}
