<script lang="ts">
  import { ethers } from "ethers";

  let account: string = "";
  let balance: string = "";
  let isConnected: boolean = false;

  async function connectWallet() {
    if (!(window as any).ethereum) {
      alert("Instala Pali Wallet");
      return;
    }

    const provider = new ethers.BrowserProvider((window as any).ethereum);

    await provider.send("eth_requestAccounts", []);

    const signer = await provider.getSigner();
    account = await signer.getAddress();

    const bal = await provider.getBalance(account);
    balance = parseFloat(ethers.formatEther(bal)).toFixed(4);

    isConnected = true;
  }

  function disconnectWallet() {
    account = "";
    balance = "";
    isConnected = false;
  }

  function shortenAddress(addr: string) {
    return addr.slice(0, 6) + "..." + addr.slice(-4);
  }
</script>

<main>
  <div class="wallet">
    
    <!-- HEADER -->
    <div class="header">
      <h2>Pali Wallet - Inicio de sesion</h2>
      {#if isConnected}
        <span class="status">● Conectado</span>
      {/if}
    </div>

    <!-- BODY -->
    <div class="content">
      {#if !isConnected}
        <button class="connect" on:click={connectWallet}>
          Conectar Wallet
        </button>
      {/if}

      {#if isConnected}
        <div class="account">
          <p class="address">{shortenAddress(account)}</p>
        </div>

        <div class="balance">
          <h1>{balance}</h1>
          <span>ETH</span>
        </div>

        <button class="disconnect" on:click={disconnectWallet}>
          Desconectar
        </button>
      {/if}
    </div>

  </div>
</main>

<style>
  main {
    background: #0d0d0d;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: 'Segoe UI', sans-serif;
  }

  .wallet {
    width: 320px;
    background: #1c1c1e;
    border-radius: 20px;
    box-shadow: 0 0 20px rgba(0,0,0,0.5);
    overflow: hidden;
    color: white;
  }

  .header {
    padding: 15px;
    background: #111;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .header h2 {
    margin: 0;
  }

  .status {
    color: #4cd964;
    font-size: 12px;
  }

  .content {
    padding: 20px;
    text-align: center;
  }

  .connect {
    width: 100%;
    padding: 12px;
    border-radius: 12px;
    border: none;
    background: #6c5ce7;
    color: white;
    font-size: 15px;
    cursor: pointer;
  }

  .connect:hover {
    background: #7d6df0;
  }

  .account {
    margin-bottom: 15px;
  }

  .address {
    font-size: 14px;
    color: #aaa;
  }

  .balance h1 {
    margin: 0;
    font-size: 40px;
  }

  .balance span {
    color: #888;
  }

  .disconnect {
    margin-top: 20px;
    width: 100%;
    padding: 10px;
    border-radius: 10px;
    border: none;
    background: #ff3b30;
    color: white;
    cursor: pointer;
  }

  .disconnect:hover {
    background: #ff5c52;
  }
</style>