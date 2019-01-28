# Tobet Happy Bull Verify

验证工具网页请访问：https://tobetone.github.io/HappyBull/

在使用此网页工具之前，请仔细阅读以下说明。你可以根据如下说明，自行开发程序验证。
## 开奖结果计算
  1. 根据游戏数据生成随机种子,并签名：  
    seed = GameId+BetPlayersCount+LastBetTime;  
    seed_sign = sign(seed)  
  2. 对随机种子hash（SH256）运算，结果转换为16进制  
    hash_hex = hex(sha256(seed_sign)) 
  3. 对hash_hex 分别截取5次6位字符转换成数字进行运算,每次进行分为庄(banker),上门(top),天门(mid),下门(bot)
  
```javascript
one = hexToInt(hash.substring(0, 6));

two = hexToInt(hash.substring(6, 12));

three = hexToInt(hash.substring(12, 18));

four = hexToInt(hash.substring(18, 24));

five = hexToInt(hash.substring(24, 30));

player = [banker, top, mid, bot]

bout = [one, two, three, four, five];

for (var i = 0; i < bout.length; i++) {

  for (var j = 0; j < player.length; j++) {

    player[j].push(poker[bout[i] % poker.length]);

    poker.splice(bout[i] % poker.length, 1);

        }
   }

bankerResult：player[0]

topResult：player[1]

midResult：player[2]

botResult：player[3]
```
## 随机因子说明
   seed = GameId+BetPlayersCount+LastBetTime
*  GameId:游戏ID
*  BetPlayersCount:参与游戏玩家个数
*  LastBetTime:最后一位投注的玩家的投注时间
## 签名验证
   验证签名使用Tobet的公钥（EOS6CG8VwJ8G1iFn6x781PMojmfD7i4kqqzsgd1AjWwAaEz35QGhn），对seed_sign进行ecc签名验证，验证结果通过即可证明随机种子未被篡改。

