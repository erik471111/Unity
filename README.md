# Unity
Basic comman for unity
Player controller:
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Playercontroller : MonoBehaviour
{


    public float speed = 5;
    public float jumpforce = 200;
    public Rigidbody rb;
    public bool ground;
    // Start is called before the first frame update
    private void OnCollisionEnter(Collision collision)
    {
        ground = true;
    }
    void Update()
        {
        var vel = new Vector3(Input.GetAxis("Horizontal"), 0, Input.GetAxis("Vertical")) * speed;
        vel.y = rb.velocity.y;
        rb.velocity = vel;
        
        if (Input.GetKeyDown(KeyCode.Space)&&(ground==true))
        {
            rb.AddForce(Vector3.up * jumpforce);
            ground = false;
        }
       
        
    }
    }
    --------
    Camera follow:
    using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Real : MonoBehaviour
{
    // Start is called before the first frame update
    public Transform cameraTarget;
    public float sSpeed = 10.0f;
    public Vector3 dist;
    public Transform lookTarget;

    void FixedUpdate()
    {
        Vector3 dPos = cameraTarget.position + dist;
        Vector3 sPos = Vector3.Lerp(transform.position, dPos, sSpeed * Time.deltaTime);
        transform.position = sPos;
        transform.LookAt(lookTarget.position);
    }

}
--------
Coin Controller:
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class CoinController : MonoBehaviour
{
    public int coinCount = 0;
    public Text cointext;
    public GameObject coinPrefab;
    public int coinMax = 15;
    public float gameTime = 30f;
    public float spawnRate = 2f;
    public Vector3 spawnRange = new Vector3(10f, 0f, 10f);

    private int collectedCoins = 0;
    private float timeLeft;

    void Start()
    {
        StartCoroutine(SpawnCoins());
        timeLeft = gameTime;
    }

    void Update()
    {
        timeLeft -= Time.deltaTime;
        if (timeLeft < 0f)
        {
            Debug.Log("Time's up! You lose!");
            // TODO: Implement losing condition
        }
    }

    IEnumerator SpawnCoins()
    {
        while (collectedCoins < coinMax)
        {
            Vector3 spawnPos = new Vector3(Random.Range(-spawnRange.x, spawnRange.x), spawnRange.y, Random.Range(-spawnRange.z, spawnRange.z));
            Instantiate(coinPrefab, spawnPos, Quaternion.identity);
            yield return new WaitForSeconds(spawnRate);
        }
    }

    void OnTriggerEnter(Collider other)
    {
        if (other.gameObject.CompareTag("coin"))
        {
            Destroy(other.gameObject); 
            coinCount++; 
            Debug.Log(coinCount);
            cointext.text = "Coins: " + coinCount.ToString();
            if (collectedCoins >= coinMax)
            {
                Debug.Log("You win!");
                
            }
        }
    }
}


______
Camera rotater:
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Camerarotate : MonoBehaviour
    
{
    private float x;
    private float y;
    public float sens = -1f;
    private Vector3 rotate;
    // Start is called before the first frame update
    void Start()
    {
        Cursor.lockState = CursorLockMode.Locked;
    }

    // Update is called once per frame
    void Update()
    {
        y = Input.GetAxis("Mouse X");
        x = Input.GetAxis("Mouse Y");
        rotate = new Vector3(x, y * sens, 0);
        transform.eulerAngles = transform.eulerAngles - rotate;

    }
}

