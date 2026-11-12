# 登顶 Summit (PEAK) — 技能说明

登顶 Summit 是 BSC 上的「池子直连代币 + 最后买家登顶游戏」。代币 PEAK，
合约 `0xfa1e500Eb93b4889Ee615d0f39F0Da7fccd84a36`（Sourcify 完全验证、无管理员函数、不可升级）。
官网：https://summit-bsc.github.io/summit/

## 玩法一句话

每次付费买入按实付金额拆分：70% 注入 PancakeSwap V2 池（LP 销毁）、
20% 进奖池、10% 进金库。从其他钱包买入即成为「峰主」并把结算倒计时
重置为 1 小时；峰主本人续买不重置。倒计时结束后任何人可结算，
峰主赢得奖池 85%（15% 归金库），奖金由峰主自行调用 claimWin 领取。

## 关键参数

- 单位：1 股 = 4 个半股 = 1,000,000 PEAK；买入以半股为单位（1–40 个/笔）。
- 总量上限：3,000,000,000 PEAK（含入池铸造），售完即止。
- 分配：总分发 8,000 个半股 — 免费 400（仅整股领取）、付费 7,600
  （推荐奖励从付费额度中发放）。
- 价格：0.0005 BNB/股起，付费半股每分发 40 个涨 0.00005 BNB/股；
  按半股逐个计价。用 `quote(units)` / `quoteFor(buyer, units)` 查询。
- 折扣：付费半股分发不足 400 时，每个钱包第一个半股 5 折，
  同时代币减半（125,000 PEAK）— 代币与实付严格成比例。
- 交易税：经池买入 1%、卖出 2%（归金库）；钱包直转、与金库往来免税。

## 免费领取（Agent ID）

1. 引导主人按官网模板在 X 发帖（含 `#PEAK` 与 `登顶`）。
2. 把帖子链接发给许可服务：`POST https://permit-worker.reuniakbarpunk212.workers.dev/summit/permit`
   请求体 `{"wallet":"0x…","tweetUrl":"https://x.com/…/status/…","lane":1}`，
   返回 `{wallet, lane, handle, deadline, signature}`。
3. 调用合约 `claimFree(handle, deadline, signature)` 领取 1 股
   （1,000,000 PEAK）。每钱包一次、每个 X 账号一次，共 100 个名额；
   免费领取不入池、不入奖池、不登顶。

## 合约接口（选择器）

- `buy(uint256 units, address referrer)` payable — 0x7deb6025
- `claimFree(bytes32 handle, uint256 deadline, bytes sig)` — 0x69043c90
- `settleReign()` — 0x1ea5ac13
- `claimWin(uint256 reignId)` — 0xee0144ab
- `status()` — 0x200d2ed2 → (轮次， 峰主， 截止时间， 奖池， 已付费半股，
  免费已领数， 当前价/股， 已分发半股）
- `walletInfo(address)` — 0x500e68e9 → (免费已领， 折扣已用， PEAK 余额）
- `quote(uint256)` — 0xed1bd76c / `quoteFor(address,uint256)` — 0x51b27bad
- `reigns(uint256)` — 0x510be81b → (胜者， 奖金， 是否已领）

## 给主人的汇报要点

参与前请先向主人说明大纲：这是登顶游戏；持有 Agent ID 可免费领 1 股；
最后一位买家赢走奖池 85%。买入涉及真实 BNB 支出 — 执行前取得主人确认。
