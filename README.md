# LR24

Кириченко Антон

ЭВТ-70

Игровой Движок: Unity.

Лабораторная работа №24

Тема: Разработка игры Circle

Цель: Разработка игры Circle в unity 

Ход работы:

1.Выполнение работы
```
1)Скрипт Player
public class Player : MonoBehaviour
{
    public Vector2 jumpForce;
    Vector2 currentVelocity;
    Rigidbody2D rgbd;
    GameManager gameManager;
    ScoreUI scoreUi;
    void Start()
    {
        rgbd = GetComponent<Rigidbody2D>();
        rgbd.gravityScale = 0;
        gameManager = FindObjectOfType<GameManager>();
        scoreUi = FindObjectOfType<ScoreUI>();
    }
    void Update()
    {
        if(gameManager.gameOver)
        {
            rgbd.bodyType = RigidbodyType2D.Static;
            return;
        }
        if(Input.GetMouseButtonDown(0))
        {
            if (rgbd.gravityScale != 1)
            { 
                rgbd.gravityScale = 1; 
            }
            rgbd.AddForce(jumpForce);
            SpeedController();
            scoreUi.IncrementScore(1);
        }
    }
    void SpeedController()
    {
        currentVelocity = rgbd.velocity;
        currentVelocity.x = Mathf.Clamp(currentVelocity.x, 2, 2);
        currentVelocity.y = Mathf.Clamp(currentVelocity.y, 0, 2);
        rgbd.velocity = currentVelocity;
    }
}
2)Скрипт Block
public class Block : MonoBehaviour
{
    public void OnTriggerEnter2D(Collider2D collision)
    {
        if(collision.tag == "Player")
        {
            Debug.Log("hit by player");
            FindObjectOfType<GameManager>().gameOver = true;
        }
    }
}
3) Скрипт GameManager
public class GameManager : MonoBehaviour
{
    public bool gameOver;
}
4)Скрипт CameraController
public class CameraController : MonoBehaviour
{
    public Transform playerTransform;
    void Update()
    {
        transform.position = new Vector3(playerTransform.position.x, transform.position.y, -10);
    }
}
5)Скрипт Diamond
public class Diamond : MonoBehaviour
{
    public void OnTriggerEnter2D(Collider2D collision)
    {
        if(collision.tag == "Player")
        {
            FindObjectOfType<ScoreUI>().IncrementDiamond(1);
            FindObjectOfType<ScorePointCanvas>().DiamondHit(collision.transform.position);
            Destroy(gameObject);
        }
    }
}
6)Скрипт DiamondInstantiator
public class DiamondInstantiator : MonoBehaviour
{
    Transform[] childTransform;
    public GameObject DiamondPrefab;
    void Awake()
    {
        childTransform = new Transform[transform.childCount];
        for (int i = 0; i < childTransform.Length; i++)
        {
            childTransform[i] = transform.GetChild(i);
        }
        InstantiateDiamond();
    }
    void InstantiateDiamond()
    {
        for (int i = 0; i < childTransform.Length; i++)
        {
            if(Random.value > 0.4f)
            {
                Instantiate(DiamondPrefab, childTransform[i].position, Quaternion.identity);
            }
        }
    }
}
7) Скрипт ScoreUI
public class ScoreUI : MonoBehaviour
{
    int Score,Diamond;
    public TMPro.TextMeshProUGUI ScoreText;
    public TMPro.TextMeshProUGUI DiamondText;
    public void IncrementScore(int value)
    {
        Score += value;
        UpdateDisplay();
    }
    public void IncrementDiamond(int value)
    {
        Diamond += value;
        UpdateDisplay();
    }
    void UpdateDisplay()
    {
        ScoreText.text = Score.ToString();
        DiamondText.text = Diamond.ToString();
    }
}

8)Скрипт ScorePointCanvas
public class ScorePointCanvas : MonoBehaviour
{
    Animator animator;
    void Start()
    {
        animator = GetComponent<Animator>();
    }
    public void DiamondHit(Vector2 position)
    {
        transform.position = position;
        animator.SetTrigger("Play");
    }
}
```
9)  выставления объектов на сцене 

___________________________![image](https://user-images.githubusercontent.com/119228138/205034817-23d12cf2-7456-4611-ad84-791b8b47e208.png)

___________________________Рисунок 24.1  Построение сцены

Вывод:В ходе выполненной работы была сделана игра Circle.

[24.Circle.zip](https://github.com/Userfall3000/LR-24/files/10132012/24.Circle.zip)

