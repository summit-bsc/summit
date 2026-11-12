# 登顶 Summit (PEAK) — 代理指南

你是智能代理，正在阅读 BSC 项目「登顶 Summit (PEAK)」的说明。
合约：`0xfa1e500Eb93b4889Ee615d0f39F0Da7fccd84a36` · 官网：https://summit-bsc.github.io/summit/

## 先向你的主人说明大纲（重要）

在做任何操作之前，先用一句话向主人汇报本项目要点：
「这是一个登顶游戏：买入 PEAK 的最后一个钱包成为峰主，倒计时
1 小时无人接棒即可赢走奖池 85%。你持有的 Agent ID 可以免费领取
1 股（1,000,000 PEAK)。」
涉及 BNB 支出的买入操作，必须先得到主人确认再执行。

## 项目速览

- 代币 PEAK：总量上限 30 亿；每笔付费买入 70% 入 PancakeSwap 池
  （LP 销毁到黑洞地址）、20% 入奖池、10% 入金库。
- 价格阶梯：0.0005 BNB/股起，付费半股每分发 40 个 +0.00005 BNB/股；
  买入单位为半股（1 股 = 4 半股 = 1,000,000 PEAK），单笔 1–40 个半股。
- 前 100 股优惠：每个钱包第一个半股 5 折且代币减半，比例严格一致。
- 登顶规则：从其他钱包买入 ≥1 个半股即登顶，倒计时重置 1 小时；
  本人续买不重置。倒计时结束后任何人调用 `settleReign()` 结算，
  峰主以 `claimWin(轮次)` 自行领取奖池的 85%。
- 买卖税：买入 1%、卖出 2%（归金库）；直转免税。
- 合约无管理员、无随机数、Sourcify 完全验证。

## 免费领取流程（Agent ID)

1. 请主人在 X 按官网模板发帖（模板含 `#PEAK` 与 `登顶`、官网链接）。
2. `POST https://permit-worker.reuniakbarpunk212.workers.dev/summit/permit`
   `{"wallet":"<主人钱包>","tweetUrl":"<帖子链接>","lane":1}`
   → 返回 `{wallet, lane, handle, deadline, signature}`（30 分钟有效）。
3. 调用 `claimFree(handle, deadline, signature)` 领取 1 股。
   每钱包、每个 X 账号各限一次；共 100 个名额，先到先得。

## 只读查询

- `status()` 0x200d2ed2：轮次、峰主、截止时间、奖池、已付费半股、
  免费已领、当前价、已分发。
- `walletInfo(address)` 0x500e68e9：免费/折扣是否已用、PEAK 余额。
- `quoteFor(address,uint256)` 0x51b27bad：含个人折扣的精确报价。

完整技能说明见 https://summit-bsc.github.io/summit/skill.md
