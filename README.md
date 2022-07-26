# Forge-Satoshi


# 缺陷分析

PoC存在以下陷阱：
•泄漏𝑘 导致d的泄漏

•重复使用𝑘 导致d的泄漏

•两个用户，使用𝑘 导致d的泄漏, 也就是说他们可以推断彼此的𝑑例如：𝑟, 𝑠 和𝑟,−𝑠 都是有效签名

•假装是佐藤，因为如果验证不正确，就可以伪造签名𝑚 • 相同的𝑑 和𝑘 用于ECDSA和Schnorr签名，导致𝑑的泄露


# 代码思路分析
# 1
泄漏𝑘 导致d的泄漏和重复使用𝑘 导致d的泄漏.


从验证公式和私钥计算公式中，我们可以简单地推导出：

D=R-1（k*s-e）mod N和R，k，s，e被敌人知道，所以可以找到。

两个签名使用相同的K。对于相同的g和P，R是相同的，并且R的值是已知的。E1和E2来自信息M1和M2。同时s计算方程：


s1 = k-1;  
(e1+dr) mod n;
s2 = k-1;
(e2+dr) mod n;

如下：



def k_Leaking(r,n,k,s,m):
    r_reverse=Gcd(r,n)
    e=hash(m)
    d=r_reverse * (k*s-e)%n
    return d
    
def k_Reuse(r1,s1,m1,r2,s2,m2,n):
    e1=hash(m1)
    e2=hash(m2)
    d=((s1 * e2 - s2 * e1) * Gcd((s2 * r1 - s1 * r1), n)) % n
    return d


# 2 推导彼此的K

def Use_the_Same_k(s1,m1,s2,m2,r,d1,d2,n):
    e1=hash(m1)
    e2=hash(m2)
    d2_1 = ((s2 * e1 - s1 * e2 + s2 * r * d1) * Gcd(s1 * r, n)) % n
    d1_1 = ((s1 * e2 - s2 * e1 + s1 * r * d2) * Gcd(s2 * r, n)) % n
    if(d2==d2_1 and d1_1==d1):
        print("Success...")
        return 1
    else:
        print("Failure...")
        return 0

# 3 𝑟, 𝑠 和𝑟,−𝑠 都是有效的签名


def Pretend(r, s, n, G, P):
    u = random.randrange(1, n - 1)
    v = random.randrange(1, n - 1)
    r1 = Add(Multiply(u, G), Multiply(v, P))[0]
    e1 = (r1 * u * Gcd(v, n)) % n
    s1 = (r1 * Gcd(v, n)) % n
    Verify_without_m(e1, n, G, r1, s1, P)



# 4 因为如果验证没有检查m，就可以伪造签名



def Pretend(r, s, n, G, P):
    u = random.randrange(1, n - 1)
    v = random.randrange(1, n - 1)
    r1 = Add(Multiply(u, G), Multiply(v, P))[0]
    e1 = (r1 * u * Gcd(v, n)) % n
    s1 = (r1 * Gcd(v, n)) % n
    Verify_without_m(e1, n, G, r1, s1, P)
    
    
    

# 结果展示

![image](https://user-images.githubusercontent.com/75195549/181039496-6e2a524c-d87b-4675-b85a-c6ce13d14925.png)
