# Forge-Satoshi


# 缺陷分析

PoC存在以下陷阱：
•泄漏𝑘 导致d的泄漏

•重复使用𝑘 导致d的泄漏

•两个用户，使用𝑘 导致d的泄漏, 也就是说他们可以推断彼此的𝑑例如：𝑟, 𝑠 和𝑟,−𝑠 都是有效签名

•假装是佐藤，因为如果验证不正确，就可以伪造签名𝑚 • 相同的𝑑 和𝑘 用于ECDSA和Schnorr签名，导致𝑑的泄露


# 代码思路分析
# 1泄漏𝑘 导致d的泄漏和重复使用𝑘 导致d的泄漏.


从验证公式和私钥计算公式中，我们可以简单地推导出：

D=R-1（k*s-e）mod N和R，k，s，e被敌人知道，所以可以找到。

两个签名使用相同的K。对于相同的g和P，R是相同的，并且R的值是已知的。E1和E2来自信息M1和M2。同时s计算方程：


s1 = k-1;  
(e1+dr) mod n;
s2 = k-1;
(e2+dr) mod n;

如下：



![image](https://user-images.githubusercontent.com/75195549/181041462-0fd88217-6512-4153-b552-b6fd29fafd38.png)


# 2 推导彼此的K

![image](https://user-images.githubusercontent.com/75195549/181041507-05497b69-cff9-477b-b6a0-bae6828a9989.png)

# 3 𝑟, 𝑠 和𝑟,−𝑠 都是有效的签名

# 4 因为如果验证没有检查m，就可以伪造签名




![image](https://user-images.githubusercontent.com/75195549/181041552-ab52ab1a-159c-4206-8396-77f9850c1d19.png)


# 5相同的𝑑 和𝑘 用于ECDSA和Schnorr签名，导致𝑑的泄露

![image](https://user-images.githubusercontent.com/75195549/181041887-824d860d-c639-4b93-83db-8d1a42e29e2b.png)

    
    

# 结果展示

![image](https://user-images.githubusercontent.com/75195549/181039496-6e2a524c-d87b-4675-b85a-c6ce13d14925.png)
