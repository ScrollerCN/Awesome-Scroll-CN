# 从Scroll L1 存款至 Scroll L2

### 使用说明

1. 首先，切换到钱包中的**Scroll L1网络。**
2. 在跨链桥应用程序中，确保**Scroll L1**在顶部，**Scroll L2**在底部。您可以点击“ **↓** ”按钮切换位置。
3. 选择您要从 Scroll L1 转移到 Scroll L2 的代币（`TSETH`或`TSUSDC`）
4. 如果这是您第一次转移代`TSUSDC`币，您需要 **批准** Scroll L1 跨链桥合约才能访问您的代`TSUSDC`币。
5. 接下来，单击 **发送** 按钮进行存款。您的钱包会弹出一个窗口，要求确认转账交易。
6. 转账交易发送并确认后，代币将从您的 Scroll L1 钱包中扣除。
    

### 代币何时会到达您的 Scroll L2 钱包？

大约需要 **2 分钟（Scroll L1 上的 6 个区块确认）**，代币在出现在您的 Scroll L2 钱包中， 您可以通过以下方式查看存款交易进度：

1. 单击跨链桥应用程序右上角的钱包地址。
	![deposit](img/deposit_1.png)
	弹出面板列出了您在跨链桥应用程序中进行的最近交易（见下图）。有两种状态：L1 状态和L2 状态。此时，因为我们是从 L1 跨至 L2 ，所以我们在将交易提交到 L1 跨链桥后会很快得到一个 `success`状态。另一方面，L2 需要大约**2 分钟**才能到达 `success`状态。
2. 点击最新的 L1 交易哈希
	![deposit](img/deposit_2.png)
	将会在新的标签页显示交易详情，您可以看到此笔交易已经在L1上得到确认
	![deposit](img/deposit_3.png)
3. 返回[跨链桥](https://scroll.io/prealpha/bridge)应用。一旦您的交易在 L2 上的状态显示`success`（**约 2 分钟后**），您应该会看到 Scroll L2 钱包中的资金和交易哈希：
	![deposit](img/deposit_4.png)