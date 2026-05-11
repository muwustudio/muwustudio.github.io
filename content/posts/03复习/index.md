---
title: "03MMORPG项目-C#复习手册"
description: "适用项目：Unity MMORPG客户端开发"
image: cover.png
date: 2026-04-02T10:00:00+08:00
tags:
  - MMORPG
  - 游戏项目
  - 笔记
categories:
  - 技术笔记
draft: false
---

# 🎮 MMORPG项目 · C# 复习手册

> 适用项目：Unity MMORPG客户端开发
> 阅读建议：按章节顺序读，重点看⭐标注的内容
> 更新日期：2025

---

# 📚 目录

- [第一章：C# 基础快速复习](#第一章c-基础快速复习)
- [第二章：面向对象核心（OOP）](#第二章面向对象核心oop)
- [第三章：Unity常用C#特性](#第三章unity常用c特性)
- [第四章：游戏开发设计模式](#第四章游戏开发设计模式)
- [第五章：项目常用数据结构速查](#第五章项目常用数据结构速查)
- [第六章：常见错误与调试技巧](#第六章常见错误与调试技巧)

---

# 第一章：C# 基础快速复习

## 1.1 变量与数据类型

```csharp
// ===== 常用数据类型 =====
int     level   = 1;          // 整数：等级、数量
float   hp      = 100.5f;     // 小数：血量、速度（注意要加f）
double  ratio   = 0.85;       // 高精度小数（较少用）
bool    isAlive = true;       // 布尔：死活、开关状态
string  name    = "战士";     // 字符串：名字、描述
char    grade   = 'A';        // 单个字符（较少用）

// ===== 类型转换 =====
int    a = 10;
float  b = (float)a;          // 强制转换：int → float
string s = a.ToString();      // 数字转字符串
int    c = int.Parse("100");  // 字符串转数字（失败会报错）
int    d = Convert.ToInt32("100"); // 更安全的转换方式
```

## 1.2 运算符

```csharp
// ===== 算术运算符 =====
int a = 10 + 3;   // 13  加
int b = 10 - 3;   // 7   减
int c = 10 * 3;   // 30  乘
int d = 10 / 3;   // 3   除（整数相除，小数部分丢弃！）
int e = 10 % 3;   // 1   取余（常用于判断奇偶、循环）

// ===== 常用快捷写法 =====
int hp = 100;
hp += 20;   // 等于 hp = hp + 20; → hp = 120
hp -= 30;   // 等于 hp = hp - 30; → hp = 90
hp++;       // 等于 hp = hp + 1;  → hp = 91
hp--;       // 等于 hp = hp - 1;  → hp = 90

// ===== 比较运算符（结果是bool）=====
bool r1 = (5 > 3);    // true
bool r2 = (5 == 5);   // true  （注意：==是比较，=是赋值）
bool r3 = (5 != 3);   // true  （不等于）
bool r4 = (5 >= 5);   // true  （大于等于）

// ===== 逻辑运算符 =====
bool r5 = (true && false);  // false  （两个都true才是true）
bool r6 = (true || false);  // true   （有一个true就是true）
bool r7 = !true;            // false  （取反）

// ===== 游戏中的实际例子 =====
// 判断玩家能否攻击（活着 且 有攻击力 且 不在冷却）
bool canAttack = isAlive && (attackPower > 0) && !isOnCooldown;
```

---

## 1.3 条件判断

```csharp
// ===== if / else if / else =====
int playerLevel = 15;

if (playerLevel >= 60)
{
    Debug.Log("满级玩家");
}
else if (playerLevel >= 30)
{
    Debug.Log("中级玩家");
}
else
{
    Debug.Log("新手玩家");
}

// ===== switch（多个固定值判断时更清晰）=====
string job = "战士";

switch (job)
{
    case "战士":
        Debug.Log("物理攻击职业");
        break;
    case "法师":
        Debug.Log("魔法攻击职业");
        break;
    case "牧师":
        Debug.Log("治疗职业");
        break;
    default:
        Debug.Log("未知职业");
        break;
}

// ===== 三元运算符（简短版if-else）=====
string status = (hp > 0) ? "存活" : "死亡";
// 等价于：
// if (hp > 0) status = "存活";
// else status = "死亡";
```

---

## 1.4 循环

```csharp
// ===== for循环（知道次数时用）=====
for (int i = 0; i < 5; i++)
{
    Debug.Log("第" + i + "次循环");   // 输出0,1,2,3,4
}

// ===== while循环（不知道次数时用）=====
int count = 0;
while (count < 3)
{
    Debug.Log("count = " + count);
    count++;   // ⚠️ 别忘了这句，否则死循环！
}

// ===== foreach循环（遍历集合时最常用）=====
List<string> itemList = new List<string> { "剑", "盾", "药水" };

foreach (string item in itemList)
{
    Debug.Log("背包里有：" + item);
}

// ===== break 和 continue =====
for (int i = 0; i < 10; i++)
{
    if (i == 3) continue;  // 跳过i=3这次，继续下一次
    if (i == 7) break;     // i=7时直接退出整个循环
    Debug.Log(i);          // 输出：0,1,2,4,5,6
}
```

---

## 1.5 方法（函数）

```csharp
// ===== 基本格式 =====
// 访问修饰符  返回类型  方法名  (参数列表)
public        int       Add    (int a, int b)
{
    return a + b;
}

// ===== 无返回值（void）=====
public void ShowMessage(string msg)
{
    Debug.Log(msg);
    // 无需return（或者写 return; 直接退出）
}

// ===== 有默认参数值 =====
public void Attack(int damage, bool isCritical = false)
{
    // isCritical有默认值false，调用时可以不传
}

// 调用方式：
Attack(100);          // isCritical = false（用默认值）
Attack(100, true);    // isCritical = true

// ===== out 参数（返回多个值）=====
public bool TryGetItem(int itemId, out string itemName)
{
    if (itemId == 1)
    {
        itemName = "铁剑";
        return true;
    }
    itemName = "";
    return false;
}

// 调用：
if (TryGetItem(1, out string name))
{
    Debug.Log("找到了：" + name);
}
```

---

# 第二章：面向对象核心（OOP）

> ⭐⭐⭐ **这一章是最重要的，游戏里所有东西都是"对象"**

## 2.1 类与对象

```csharp
// ===== 类的定义（模板/蓝图）=====
public class Item
{
    // --- 字段（Field）：存储数据 ---
    private int    id;
    private string itemName;
    private int    count;
    
    // --- 属性（Property）：保护字段，控制读写 ---
    public int Id
    {
        get { return id; }           // 允许读
        // 注意：没有set，外部不能修改Id
    }
    
    public string ItemName
    {
        get { return itemName; }
        set { itemName = value; }    // 允许读写
    }
    
    public int Count
    {
        get { return count; }
        set
        {
            if (value < 0) value = 0;  // 数量不能为负
            count = value;
        }
    }
    
    // --- 构造函数（new的时候执行）---
    public Item(int id, string name, int count)
    {
        this.id       = id;
        this.itemName = name;
        this.count    = count;
    }
    
    // --- 方法 ---
    public void Use()
    {
        if (count <= 0)
        {
            Debug.Log("道具数量不足！");
            return;
        }
        count--;
        Debug.Log($"使用了{itemName}，剩余{count}个");
    }
    
    // 字符串插值：$"文字{变量}文字" 比 "文字" + 变量 更方便
}

// ===== 使用对象 =====
Item sword = new Item(1001, "铁剑", 1);
Item potion = new Item(2001, "红药水", 10);

sword.Use();
Debug.Log(potion.Count);  // 10
```

---

## 2.2 继承（Inheritance）

```csharp
// ===== 父类（基类）=====
public class Character
{
    public string Name     { get; protected set; }
    public int    MaxHP    { get; protected set; }
    public int    CurrentHP{ get; protected set; }
    public int    AttackPower { get; protected set; }

    public Character(string name, int maxHP, int attackPower)
    {
        Name        = name;
        MaxHP       = maxHP;
        CurrentHP   = maxHP;
        AttackPower = attackPower;
    }

    // virtual 表示子类可以重写这个方法
    public virtual void Attack(Character target)
    {
        target.TakeDamage(AttackPower);
    }

    public virtual void TakeDamage(int damage)
    {
        CurrentHP -= damage;
        if (CurrentHP < 0) CurrentHP = 0;
        Debug.Log($"{Name} 受到 {damage} 点伤害，剩余HP：{CurrentHP}");
    }
}

// ===== 子类（派生类）=====
public class Warrior : Character
{
    public int Armor { get; private set; }   // 战士独有：护甲

    // base(...) 调用父类构造函数
    public Warrior(string name) : base(name, 1000, 150)
    {
        Armor = 50;
    }

    // override 重写父类方法
    public override void TakeDamage(int damage)
    {
        int realDamage = Mathf.Max(0, damage - Armor); // 护甲减伤
        base.TakeDamage(realDamage);  // 调用父类的TakeDamage
    }

    // 战士专属技能
    public void ShieldBash(Character target)
    {
        target.TakeDamage(AttackPower / 2);
        Debug.Log($"{Name} 使用了盾击！");
    }
}

public class Mage : Character
{
    public int Mana { get; private set; }    // 法师独有：魔力

    public Mage(string name) : base(name, 600, 300)
    {
        Mana = 500;
    }

    public override void Attack(Character target)
    {
        // 法师攻击消耗魔力
        if (Mana < 20)
        {
            Debug.Log("魔力不足！");
            return;
        }
        Mana -= 20;
        target.TakeDamage(AttackPower);
    }
}

// ===== 多态：用父类类型装各种子类 =====
Character player1 = new Warrior("张三");
Character player2 = new Mage("李四");

// 调用同一个方法，但执行的是各自重写后的版本
player1.TakeDamage(100);  // 战士：减去护甲后受伤
player2.TakeDamage(100);  // 法师：直接受100伤
```

---

## 2.3 接口（Interface）

```csharp
// ===== 接口：定义"能做什么"的规范 =====

// 可以受伤的接口
public interface IDamageable
{
    void TakeDamage(int damage);
    int  CurrentHP { get; }
}

// 可以被交互的接口（NPC、宝箱等）
public interface IInteractable
{
    void Interact(GameObject player);
    string GetInteractPrompt();   // 返回提示文字，如"按F交互"
}

// 可以被拾取的接口
public interface IPickable
{
    void OnPickup(GameObject player);
}

// ===== 实现接口 =====
// 类可以同时实现多个接口
public class Monster : MonoBehaviour, IDamageable
{
    public int CurrentHP { get; private set; } = 200;

    public void TakeDamage(int damage)
    {
        CurrentHP -= damage;
        if (CurrentHP <= 0)
        {
            Die();
        }
    }

    private void Die()
    {
        Debug.Log("怪物死亡！");
        Destroy(gameObject);
    }
}

public class DroppedItem : MonoBehaviour, IInteractable, IPickable
{
    private string itemName = "金币";

    public string GetInteractPrompt()
    {
        return $"按F拾取{itemName}";
    }

    public void Interact(GameObject player)
    {
        OnPickup(player);
    }

    public void OnPickup(GameObject player)
    {
        Debug.Log($"{player.name} 拾取了 {itemName}");
        Destroy(gameObject);
    }
}

// ===== 接口的好处：统一处理不同类型 =====
// 不管是玩家还是怪物，只要实现了IDamageable，都可以这样处理：
public void DealDamage(IDamageable target, int damage)
{
    target.TakeDamage(damage);  // 不用管target是玩家还是怪物
}
```

---

## 2.4 访问修饰符速查

| 修饰符             | 能访问的地方                   | 游戏开发用途         |
| ------------------ | ------------------------------ | -------------------- |
| `public`           | 任何地方                       | 对外公开的属性和方法 |
| `private`          | 只有自己类内部                 | 保护内部数据         |
| `protected`        | 自己和子类                     | 父类给子类用的方法   |
| `internal`         | 同一个程序集                   | 较少用               |
| `[SerializeField]` | Unity特有：私有但Inspector可见 | **最常用！**         |

```csharp
public class PlayerStats : MonoBehaviour
{
    public  string playerName;         // Inspector可见，外部可访问
    private int    secretData;         // Inspector不可见，外部不可访问
    
    [SerializeField]
    private int hp = 100;              // Inspector可见（方便调试），但外部不能改
    
    [SerializeField]
    private GameObject weaponPrefab;   // 在Inspector拖入预制体，脚本里private保护
}
```

---

# 第三章：Unity常用C#特性

## 3.1 MonoBehaviour 生命周期

```
游戏运行顺序：
Awake → OnEnable → Start → Update(每帧) → OnDisable → OnDestroy
                            FixedUpdate(物理帧)
                            LateUpdate(Update之后)
```

```csharp
public class PlayerController : MonoBehaviour
{
    // 1. Awake：最先执行，对象激活时立即调用
    //    用途：初始化自身组件引用（GetComponent）
    void Awake()
    {
        rb = GetComponent<Rigidbody>();  // 获取组件
    }
    
    // 2. Start：Awake之后，第一帧Update之前执行一次
    //    用途：依赖其他对象的初始化（其他对象的Awake已执行完）
    void Start()
    {
        GameManager.Instance.RegisterPlayer(this); // 注册到管理器
    }
    
    // 3. Update：每帧执行（帧率不稳定，60fps时每秒约60次）
    //    用途：处理输入、更新逻辑、检测状态
    void Update()
    {
        HandleInput();    // 处理玩家输入
        UpdateCooldown(); // 更新冷却时间
    }
    
    // 4. FixedUpdate：固定时间间隔执行（默认0.02秒/次，与帧率无关）
    //    用途：物理相关（移动Rigidbody、施加力）
    void FixedUpdate()
    {
        MoveWithPhysics(); // 物理移动
    }
    
    // 5. LateUpdate：所有Update执行完后执行
    //    用途：摄像机跟随（确保在玩家移动后再跟随）
    void LateUpdate()
    {
        UpdateCamera(); // 更新摄像机位置
    }
    
    // 6. OnDestroy：对象销毁时调用
    //    用途：取消事件订阅、释放资源
    void OnDestroy()
    {
        GameManager.Instance.UnregisterPlayer(this);
    }
}
```

> ⚠️ **常见错误**：在 `Awake` 里访问其他对象的组件可能为null（那个对象的Awake还没跑）
> ✅ **正确做法**：自己的组件在 `Awake` 获取，依赖别人的在 `Start` 里操作

---

## 3.2 协程（Coroutine）

> 协程 = 可以"暂停等待"的方法，不会阻塞主线程

```csharp
// ===== 基本用法 =====
public class SkillManager : MonoBehaviour
{
    // 协程方法：返回类型必须是 IEnumerator
    IEnumerator SkillCooldown(float cooldownTime)
    {
        isOnCooldown = true;
        
        yield return new WaitForSeconds(cooldownTime); // ⏸ 暂停等待N秒
        
        isOnCooldown = false;
        Debug.Log("冷却完毕！");
    }
    
    void UseSkill()
    {
        if (isOnCooldown) return;
        
        // 执行技能效果...
        
        StartCoroutine(SkillCooldown(5f)); // 启动协程
    }
}

// ===== 常用的 yield return =====

yield return null;                          // 等待下一帧
yield return new WaitForSeconds(2f);        // 等待2秒
yield return new WaitForFixedUpdate();      // 等待下一次FixedUpdate
yield return new WaitUntil(() => isReady);  // 等到isReady为true

// ===== 在场景切换Loading中的用法 =====
IEnumerator LoadSceneWithProgress(string sceneName)
{
    AsyncOperation operation = SceneManager.LoadSceneAsync(sceneName);
    operation.allowSceneActivation = false;  // 加载完不立刻切换
    
    while (operation.progress < 0.9f)  // 0.9f是Unity异步加载的最大进度
    {
        float progress = operation.progress / 0.9f;  // 转换成0~1
        loadingBar.fillAmount = progress;             // 更新进度条
        yield return null;                            // 等下一帧继续检查
    }
    
    loadingBar.fillAmount = 1f;
    yield return new WaitForSeconds(0.5f);  // 等0.5秒让玩家看到100%
    
    operation.allowSceneActivation = true;  // 允许切换场景
}

// ===== 停止协程 =====
Coroutine myCoroutine = StartCoroutine(SkillCooldown(5f));
StopCoroutine(myCoroutine);  // 停止指定协程
StopAllCoroutines();         // 停止所有协程（危险，慎用）
```

---

## 3.3 委托与事件（Event）

> 事件 = 一种通知机制，"某事发生了，通知所有关心这件事的人"

```csharp
// ===== Action（最常用的委托类型）=====

// Action          → 无参数无返回值
// Action<T>       → 有1个参数，无返回值
// Action<T1,T2>   → 有2个参数，无返回值
// Func<T>         → 无参数，有返回值T
// Func<T1,T2,R>   → 有参数，有返回值R

// ===== 游戏中的事件系统实例 =====
public class PlayerHealth : MonoBehaviour
{
    // 声明事件
    public event Action<int, int> OnHPChanged;    // 参数：当前HP，最大HP
    public event Action           OnPlayerDied;    // 玩家死亡事件

    private int maxHP     = 100;
    private int currentHP = 100;

    public void TakeDamage(int damage)
    {
        currentHP -= damage;
        currentHP  = Mathf.Clamp(currentHP, 0, maxHP); // 钳制在0~maxHP之间
        
        // 触发事件，通知所有订阅者
        OnHPChanged?.Invoke(currentHP, maxHP);  // ?. 表示如果没人订阅就不执行
        
        if (currentHP <= 0)
        {
            OnPlayerDied?.Invoke();
        }
    }

    public void Heal(int amount)
    {
        currentHP += amount;
        currentHP  = Mathf.Clamp(currentHP, 0, maxHP);
        OnHPChanged?.Invoke(currentHP, maxHP);
    }
}

// ===== UI监听事件 =====
public class HpBarUI : MonoBehaviour
{
    [SerializeField] private Slider   hpSlider;
    [SerializeField] private Text     hpText;
    private PlayerHealth playerHealth;

    void Start()
    {
        playerHealth = FindObjectOfType<PlayerHealth>();
        
        // 订阅事件（+=）
        playerHealth.OnHPChanged += UpdateHpBar;
        playerHealth.OnPlayerDied += ShowDeathScreen;
    }

    void UpdateHpBar(int current, int max)
    {
        hpSlider.value = (float)current / max;
        hpText.text    = $"{current}/{max}";
    }

    void ShowDeathScreen()
    {
        // 显示死亡界面
    }

    void OnDestroy()
    {
        // ⚠️ 重要：取消订阅，防止内存泄漏
        if (playerHealth != null)
        {
            playerHealth.OnHPChanged -= UpdateHpBar;
            playerHealth.OnPlayerDied -= ShowDeathScreen;
        }
    }
}
```

---

## 3.4 常用Unity API速查

```csharp
// ===== GameObject操作 =====
gameObject.SetActive(false);                      // 隐藏/禁用对象
gameObject.SetActive(true);                       // 显示/启用对象
string name = gameObject.name;                    // 获取对象名称
gameObject.tag = "Enemy";                         // 设置标签

// ===== 查找对象 =====
GameObject obj  = GameObject.Find("PlayerName");          // 按名字找（性能差，少用）
GameObject obj2 = GameObject.FindWithTag("Enemy");        // 按标签找
Enemy[] enemies = FindObjectsOfType<Enemy>();             // 找所有Enemy组件

// ===== 获取组件 =====
Rigidbody    rb   = GetComponent<Rigidbody>();            // 获取自身组件
Animator     anim = GetComponent<Animator>();
AudioSource  au   = GetComponentInChildren<AudioSource>(); // 在子对象中找
Canvas       cv   = GetComponentInParent<Canvas>();        // 在父对象中找

// ===== 实例化和销毁 =====
GameObject enemy  = Instantiate(enemyPrefab);                          // 实例化
GameObject enemy2 = Instantiate(enemyPrefab, spawnPos, Quaternion.identity); // 在指定位置实例化
Destroy(enemy);          // 立即销毁
Destroy(enemy, 3f);      // 3秒后销毁
DontDestroyOnLoad(this); // 切换场景不销毁（常用于管理器）

// ===== 向量和位置 =====
Vector3 pos = transform.position;              // 获取位置
transform.position = new Vector3(0, 0, 0);    // 设置位置
transform.Translate(Vector3.forward * speed * Time.deltaTime); // 向前移动

// Time.deltaTime：上一帧到这一帧的时间差（让移动速度与帧率无关）

// ===== 调试 =====
Debug.Log("普通信息");
Debug.LogWarning("警告信息");
Debug.LogError("错误信息");
Debug.Log($"玩家HP：{currentHP}，位置：{transform.position}");
```

---

# 第四章：游戏开发设计模式

## 4.1 单例模式（Singleton）⭐⭐⭐

> **用途**：全局唯一的管理器（游戏管理器、UI管理器、音频管理器）
> 你的项目里已有 `MonoSingleton` 基类，直接继承就能用！

```csharp
// ===== 你项目里已有的MonoSingleton基类（理解原理）=====
public class MonoSingleton<T> : MonoBehaviour where T : MonoBehaviour
{
    private static T _instance;

    public static T Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = FindObjectOfType<T>();
            }
            return _instance;
        }
    }

    protected virtual void Awake()
    {
        if (_instance == null)
        {
            _instance = this as T;
            DontDestroyOnLoad(gameObject); // 切换场景不销毁
        }
        else
        {
            Destroy(gameObject); // 已有实例，销毁多余的
        }
    }
}

// ===== 直接继承使用 =====
public class AudioManager : MonoSingleton<AudioManager>
{
    [SerializeField] private AudioSource bgmSource;
    [SerializeField] private AudioSource sfxSource;

    public void PlayBGM(AudioClip clip)
    {
        bgmSource.clip = clip;
        bgmSource.Play();
    }

    public void PlaySFX(AudioClip clip)
    {
        sfxSource.PlayOneShot(clip);
    }

    public void SetBGMVolume(float volume)
    {
        bgmSource.volume = Mathf.Clamp01(volume);
    }
}

// ===== 任何地方都能调用 =====
AudioManager.Instance.PlayBGM(mainCityBGM);
AudioManager.Instance.PlaySFX(attackSound);
```

---

## 4.2 状态机（State Machine）⭐⭐⭐

> **用途**：玩家动作状态、怪物AI、任务状态

```csharp
// ===== 方法一：简单枚举状态机（推荐新手使用）=====

public enum PlayerState
{
    Idle,       // 待机
    Running,    // 跑步
    Attacking,  // 攻击
    Casting,    // 释放技能
    Dead        // 死亡
}

public class PlayerController : MonoBehaviour
{
    private PlayerState currentState = PlayerState.Idle;
    private Animator animator;

    void Awake()
    {
        animator = GetComponent<Animator>();
    }

    void Update()
    {
        switch (currentState)
        {
            case PlayerState.Idle:
                // 待机时检测输入
                if (Input.anyKey) ChangeState(PlayerState.Running);
                break;
                
            case PlayerState.Running:
                HandleMovement();
                if (!Input.anyKey) ChangeState(PlayerState.Idle);
                if (Input.GetMouseButtonDown(0)) ChangeState(PlayerState.Attacking);
                break;
                
            case PlayerState.Attacking:
                // 攻击动画由Animator控制，这里等动画结束
                break;
                
            case PlayerState.Dead:
                // 死亡状态不响应任何输入
                break;
        }
    }

    public void ChangeState(PlayerState newState)
    {
        if (currentState == newState) return;         // 同一状态不重复切换
        if (currentState == PlayerState.Dead) return; // 死了不能切换状态

        // 退出旧状态
        OnExitState(currentState);
        
        currentState = newState;
        
        // 进入新状态
        OnEnterState(newState);
    }

    private void OnEnterState(PlayerState state)
    {
        switch (state)
        {
            case PlayerState.Idle:
                animator.SetTrigger("Idle");
                break;
            case PlayerState.Attacking:
                animator.SetTrigger("Attack");
                break;
            case PlayerState.Dead:
                animator.SetTrigger("Death");
                // 启动死亡后处理协程
                StartCoroutine(HandleDeath());
                break;
        }
    }

    private void OnExitState(PlayerState state)
    {
        // 退出某个状态时需要做的清理
    }

    IEnumerator HandleDeath()
    {
        yield return new WaitForSeconds(2f);
        // 显示复活界面等操作
    }
}
```

---

## 4.3 对象池（Object Pool）⭐⭐⭐

> **用途**：怪物生成、技能特效、掉落物品——频繁 Instantiate/Destroy 会卡顿！

```csharp
// ===== 通用对象池 =====
public class ObjectPool : MonoSingleton<ObjectPool>
{
    // 每种预制体对应一个队列
    private Dictionary<string, Queue<GameObject>> poolDictionary
        = new Dictionary<string, Queue<GameObject>>();

    // 从池中取出对象
    public GameObject Get(GameObject prefab, Vector3 position, Quaternion rotation)
    {
        string key = prefab.name;

        if (!poolDictionary.ContainsKey(key))
        {
            poolDictionary[key] = new Queue<GameObject>();
        }

        GameObject obj;
        Queue<GameObject> queue = poolDictionary[key];

        if (queue.Count > 0)
        {
            obj = queue.Dequeue();          // 从池中取出
        }
        else
        {
            obj = Instantiate(prefab);      // 池空了才创建新的
            obj.name = key;                 // 去掉"(Clone)"后缀，保持key一致
        }

        obj.transform.position = position;
        obj.transform.rotation = rotation;
        obj.SetActive(true);

        return obj;
    }

    // 归还对象到池
    public void Return(GameObject obj, float delay = 0f)
    {
        if (delay > 0)
            StartCoroutine(ReturnAfterDelay(obj, delay));
        else
            ReturnImmediate(obj);
    }

    private void ReturnImmediate(GameObject obj)
    {
        string key = obj.name;

        if (!poolDictionary.ContainsKey(key))
            poolDictionary[key] = new Queue<GameObject>();

        obj.SetActive(false);
        poolDictionary[key].Enqueue(obj);  // 放回池中
    }

    IEnumerator ReturnAfterDelay(GameObject obj, float delay)
    {
        yield return new WaitForSeconds(delay);
        ReturnImmediate(obj);
    }
}

// ===== 使用示例 =====
public class MonsterSpawner : MonoBehaviour
{
    [SerializeField] private GameObject monsterPrefab;
    [SerializeField] private Transform[] spawnPoints;

    void Start()
    {
        InvokeRepeating(nameof(SpawnMonster), 1f, 5f); // 1秒后开始，每5秒生成一只
    }

    void SpawnMonster()
    {
        Transform spawnPoint = spawnPoints[Random.Range(0, spawnPoints.Length)];
        GameObject monster = ObjectPool.Instance.Get(
            monsterPrefab,
            spawnPoint.position,
            Quaternion.identity
        );
    }
}

// 怪物死亡时（归还给对象池而不是Destroy）
public class Monster : MonoBehaviour, IDamageable
{
    public void Die()
    {
        // 播放死亡效果...
        ObjectPool.Instance.Return(gameObject, 2f); // 2秒后归还到池
    }
}
```

---

## 4.4 ScriptableObject 数据配置 ⭐⭐⭐

> **用途**：道具数据、怪物数据、技能数据——不用把数据写死在代码里！

```csharp
// ===== 定义道具数据 =====
[CreateAssetMenu(fileName = "NewItem", menuName = "GameData/Item")]
public class ItemData : ScriptableObject
{
    public int    itemId;
    public string itemName;
    public string description;
    public Sprite icon;
    public int    price;
    public ItemType itemType;  // 枚举类型
    
    [Header("装备属性（装备类型才填）")]
    public int attackBonus;
    public int defenseBonus;
    public int hpBonus;
}

public enum ItemType
{
    Weapon,      // 武器
    Armor,       // 防具
    Consumable,  // 消耗品（药水等）
    Quest,       // 任务物品
    Material     // 材料
}

// ===== 使用方式（在Inspector中拖入）=====
public class ItemManager : MonoSingleton<ItemManager>
{
    [SerializeField] private ItemData[] allItems;  // 在Inspector拖入所有道具数据
    
    private Dictionary<int, ItemData> itemDatabase = new Dictionary<int, ItemData>();
    
    protected override void Awake()
    {
        base.Awake();
        // 构建快速查找字典
        foreach (ItemData item in allItems)
        {
            itemDatabase[item.itemId] = item;
        }
    }
    
    public ItemData GetItemById(int id)
    {
        if (itemDatabase.TryGetValue(id, out ItemData data))
            return data;
        
        Debug.LogWarning($"找不到ID为{id}的道具！");
        return null;
    }
}
```

---

## 4.5 观察者模式（用于任务系统等）⭐⭐

> **用途**：任务系统监听游戏事件（杀怪、拾取、对话触发任务完成）

```csharp
// ===== 简单事件总线 =====
public class EventCenter : MonoSingleton<EventCenter>
{
    private Dictionary<string, Action<object>> eventDictionary
        = new Dictionary<string, Action<object>>();

    // 订阅事件
    public void Subscribe(string eventName, Action<object> callback)
    {
        if (!eventDictionary.ContainsKey(eventName))
            eventDictionary[eventName] = null;
        
        eventDictionary[eventName] += callback;
    }

    // 取消订阅
    public void Unsubscribe(string eventName, Action<object> callback)
    {
        if (eventDictionary.ContainsKey(eventName))
            eventDictionary[eventName] -= callback;
    }

    // 触发事件
    public void Publish(string eventName, object data = null)
    {
        if (eventDictionary.ContainsKey(eventName))
            eventDictionary[eventName]?.Invoke(data);
    }
}

// ===== 使用：怪物死亡触发任务进度 =====
public class Monster : MonoBehaviour
{
    [SerializeField] private string monsterType = "哥布林";

    public void Die()
    {
        // 怪物死亡时发布事件
        EventCenter.Instance.Publish("MonsterKilled", monsterType);
        ObjectPool.Instance.Return(gameObject, 2f);
    }
}

// 任务系统监听杀怪事件
public class KillQuestTask : MonoBehaviour
{
    [SerializeField] private string targetMonster = "哥布林";
    [SerializeField] private int    requiredCount = 10;
    private int currentCount = 0;

    void OnEnable()
    {
        EventCenter.Instance.Subscribe("MonsterKilled", OnMonsterKilled);
    }

    void OnDisable()
    {
        EventCenter.Instance.Unsubscribe("MonsterKilled", OnMonsterKilled);
    }

    private void OnMonsterKilled(object data)
    {
        string killedType = data as string;
        if (killedType == targetMonster)
        {
            currentCount++;
            Debug.Log($"击杀进度：{currentCount}/{requiredCount}");
            
            if (currentCount >= requiredCount)
            {
                Debug.Log("任务完成！");
                EventCenter.Instance.Publish("QuestCompleted", this);
            }
        }
    }
}
```

---

# 第五章：项目常用数据结构速查

## 5.1 List\<T\>（列表）

```csharp
// 声明
List<string> inventory = new List<string>();

// 增
inventory.Add("铁剑");                        // 添加到末尾
inventory.Insert(0, "木盾");                  // 插入到指定位置

// 删
inventory.Remove("铁剑");                     // 删除指定元素
inventory.RemoveAt(0);                        // 删除指定索引的元素
inventory.Clear();                            // 清空

// 查
bool hasItem  = inventory.Contains("铁剑");   // 是否包含
int  index    = inventory.IndexOf("铁剑");    // 查找索引（没有返回-1）
int  count    = inventory.Count;              // 数量

// 遍历
foreach (string item in inventory)
{
    Debug.Log(item);
}

// 排序（需要元素实现IComparable，或传入比较器）
inventory.Sort();
```

---

## 5.2 Dictionary\<TKey, TValue\>（字典）

```csharp
// 背包：itemId → 数量
Dictionary<int, int> bag = new Dictionary<int, int>();

// 增/改（同一个Key赋值两次=覆盖）
bag[1001] = 5;     // 铁剑 5把
bag[2001] = 10;    // 红药水 10个

// 安全读取（用TryGetValue，避免KeyNotFoundException）
if (bag.TryGetValue(1001, out int count))
{
    Debug.Log($"铁剑数量：{count}");
}
else
{
    Debug.Log("没有这个物品");
}

// 删
bag.Remove(1001);

// 检查
bool hasItem = bag.ContainsKey(1001);
bool hasValue = bag.ContainsValue(5);

// 遍历
foreach (KeyValuePair<int, int> kvp in bag)
{
    Debug.Log($"物品ID:{kvp.Key}，数量:{kvp.Value}");
}

// 只遍历Key或Value
foreach (int id in bag.Keys)   { }
foreach (int qty in bag.Values){ }
```

---

## 5.3 Queue\<T\>（队列）和 Stack\<T\>（栈）

```csharp
// Queue：先进先出（FIFO）→ 对象池、消息队列
Queue<GameObject> bulletPool = new Queue<GameObject>();
bulletPool.Enqueue(newBullet);    // 入队
GameObject bullet = bulletPool.Dequeue();  // 出队（取出最早进入的）
int count = bulletPool.Count;

// Stack：后进先出（LIFO）→ 撤销系统、UI页面栈
Stack<string> pageHistory = new Stack<string>();
pageHistory.Push("MainMenu");    // 入栈
pageHistory.Push("Bag");
string current = pageHistory.Pop();  // 出栈 → "Bag"
string top = pageHistory.Peek();     // 查看栈顶但不出栈 → "MainMenu"
```

---

# 第六章：常见错误与调试技巧

## 6.1 最常见的报错

```
NullReferenceException: Object reference not set to an instance of an object
```

**原因**：访问了一个为null的对象

```csharp
// ❌ 错误
GameObject player;
player.SetActive(false);   // player是null！

// ✅ 正确：先检查
if (player != null)
    player.SetActive(false);

// ✅ 更优雅：空值条件运算符
player?.SetActive(false);  // player为null时不执行，不报错
```

---

```
MissingReferenceException: The object has been destroyed but you are still trying to access it.
```

**原因**：对象已被Destroy，但还持有引用

```csharp
// ✅ 检查方式
if (enemy != null)  // Unity重写了==运算符，Destroy后判null会返回true
    enemy.TakeDamage(10);
```

---

## 6.2 字符串拼接的几种方式

```csharp
string name  = "张三";
int    level = 10;

// 方式1：+ 拼接（少量时用）
string s1 = "玩家" + name + "等级" + level;

// 方式2：字符串插值（推荐，最清晰）⭐
string s2 = $"玩家{name}等级{level}";

// 方式3：string.Format（老写法，少用）
string s3 = string.Format("玩家{0}等级{1}", name, level);

// 方式4：StringBuilder（大量拼接时用，性能好）
StringBuilder sb = new StringBuilder();
sb.Append("玩家");
sb.Append(name);
sb.AppendLine($"等级{level}");  // 追加并换行
string result = sb.ToString();
```

---

## 6.3 调试技巧

```csharp
// 1. 打印变量值
Debug.Log($"当前HP：{hp}，位置：{transform.position}");

// 2. 在Scene视图画线（可视化调试）
Debug.DrawLine(start, end, Color.red, 2f);   // 画线，显示2秒
Debug.DrawRay(origin, direction, Color.green); // 画射线

// 3. 条件断言（不满足时报错）
Debug.Assert(hp > 0, "HP不应该为0！");

// 4. 给Log加标签（方便过滤）
Debug.Log("[背包系统] 添加道具：" + itemName);
Debug.Log("[战斗系统] 造成伤害：" + damage);
```

---

## 6.4 常用的 Mathf 工具方法

```csharp
// 钳制（让值保持在范围内）
int  clampedHP  = Mathf.Clamp(hp, 0, maxHP);        // 限制在 0~maxHP
float clampedV  = Mathf.Clamp01(value);              // 限制在 0~1（进度条常用）

// 最大/最小值
int  damage     = Mathf.Max(0, rawDamage - armor);   // 伤害不为负
float minDist   = Mathf.Min(dist1, dist2);

// 绝对值、平方根
float absValue  = Mathf.Abs(-5f);    // 5
float sqrtValue = Mathf.Sqrt(16f);   // 4

// 插值（常用于平滑移动/渐变）
float current = Mathf.Lerp(0f, 100f, 0.5f);  // 50（在0和100之间取50%的位置）
// UI血条平滑变化：
hpSlider.value = Mathf.Lerp(hpSlider.value, targetValue, Time.deltaTime * 5f);

// 随机数
int   randInt   = Random.Range(0, 10);    // [0, 10) 整数，不含10
float randFloat = Random.Range(0f, 1f);   // [0, 1] 小数，含1
```

---

# 📌 快速参考卡片

## C#关键字速查

| 关键字     | 用途                 | 示例                          |
| ---------- | -------------------- | ----------------------------- |
| `var`      | 自动推断类型         | `var list = new List<int>();` |
| `null`     | 空引用               | `if (obj == null)`            |
| `this`     | 当前对象自身         | `this.name = name;`           |
| `base`     | 父类                 | `base.Awake();`               |
| `static`   | 静态（不需要实例）   | `static int count;`           |
| `readonly` | 只读（赋值后不可改） | `readonly int MAX = 100;`     |
| `const`    | 常量（编译时确定）   | `const float PI = 3.14f;`     |
| `new`      | 创建对象             | `new List<int>()`             |
| `as`       | 安全类型转换         | `var e = obj as Enemy;`       |
| `is`       | 类型判断             | `if (obj is Enemy enemy)`     |
| `override` | 重写父类方法         | `override void Update()`      |
| `virtual`  | 可被重写的方法       | `virtual void Attack()`       |
| `abstract` | 抽象（子类必须实现） | `abstract void Die();`        |
| `sealed`   | 不可被继承           | `sealed class Item`           |

---

## Unity 特性标签速查

```csharp
[SerializeField]              // private变量在Inspector显示
[HideInInspector]             // public变量在Inspector隐藏
[Header("战斗属性")]          // 在Inspector显示分组标题
[Tooltip("玩家的最大血量")]   // 鼠标悬停时显示提示
[Range(0, 100)]               // Inspector显示滑动条
[RequireComponent(typeof(Rigidbody))] // 强制要求同时有Rigidbody组件
[DisallowMultipleComponent]   // 同一对象只能挂一个此组件
[CreateAssetMenu(fileName = "x", menuName = "y")] // 创建ScriptableObject菜单
```

---

*📖 本手册持续更新中，遇到新知识点随时补充！*
*🎮 加油！一起把MMORPG项目拿下！*