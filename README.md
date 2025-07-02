<B>Портфолио</B>


<B>[1]</B>Пошаговая стратегия в постапокалиптическом сетинге "Wastelands".
Основные реализованные механики:

1)Перемещение\приближение карты с помощью мыши.

2)Покупка юнитов и улучшений полей.

3)Кнопка следующего хода, обновляющая очки действия юнитов и монетки.

4)Открытие карты путем перемещения юнита.

https://disk.yandex.ru/d/gzewyF8StKbQjA

![stratego](https://github.com/user-attachments/assets/69340426-ffe3-4791-ad4a-581ae4ae03d3)
<p>Пример кода:</p>

``` ruby
 public void GenerateMap(int _HighersKey)
    {
     for(int i = 0; i < MapSize; i++)
        {
            for(int k = 0; k < MapSize; k++)
            {
                fieldNumbers[i, k] = Random.Range(0, 5);
                fieldHosts[i, k] = -1;
                clouds[i, k] = true;
            }
            
        }
    }

 public void BuildMap()
    {
        int u = 0,chance ;
        for (int i = 0; i < MapSize; i++)
        {
            for (int k = 0; k < MapSize; k++)
            {
                GameObject blok = Instantiate(fieldsBloks[fieldNumbers[i, k]], pos, Quaternion.identity);
                if (fieldNumbers[i, k] != 5)
                {
                    blok.gameObject.tag = "Terain";
                    blok.GetComponent<TerainLogic>().position[0] = i;
                    blok.GetComponent<TerainLogic>().position[1] = k;
                }
                blok.transform.parent = MapContainer;

                if (fieldNumbers[i, k] == 1)
                {
                    chance = Random.Range(0, 10);
                    if (chance <= 4)
                    {
                        GameObject res =  Instantiate(Resources[0], pos, Quaternion.identity);
                        res.GetComponent<Resources>().position[0] = i;
                        res.GetComponent<Resources>().position[1] = k;
                        res.transform.parent = ObjectCou.transform;
                    }
                }
                if (fieldNumbers[i, k] == 5 )
                {
                    BaseLogic BL = blok.GetComponent<BaseLogic>();
                    BL.position[0] = i;
                    BL.position[1] = k;
                    BL.BaseCounter = BaseListSize;
                    BL.GL = this;
                    if (u == 0)
                    {
                        blok.gameObject.tag = "PlayerBase";
                        clouds[i-1,k-1] = false; clouds[i - 1, k] = false; clouds[i - 1, k + 1] = false; clouds[i , k - 1] = false; clouds[i , k ] = false; clouds[i , k + 1] = false; clouds[i + 1, k - 1] = false; clouds[i + 1, k] = false; clouds[i + 1, k + 1] = false;
                    }
                    BL.PlayerNum = u;
                    //u++;
                    Bases.Add(blok);
                    BaseListSize++;
                }
                pos.x += 1.6f;
            }
            pos.x = 0f;
            pos.z += 1.6f;
        }
        pos.x = 0;
        pos.z = 0;
    }
```

<p>


  
</p>
<B>[2]</B>Коридорный шутер в жанре <рогалик> "Alone in the darknes".
Основные реализованные механики:

1)Случайная генерация карты и противников.

2)Накопление ресурсов между уровнями и трата их на апгрейд персонажей.

3)Подбирание/cмена/покупка оружия. Различные типы оружия и патронов.

4)Управление как под пк так и под андройд.

https://disk.yandex.ru/d/jUlAnDcddATiKA

![shut](https://github.com/user-attachments/assets/e7625bb1-a594-47cb-ba46-d874c2ac7815)
<p>Пример кода:</p>

``` ruby
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.EventSystems;

public class jostic : MonoBehaviour,IDragHandler,IPointerUpHandler,IPointerDownHandler
{
	private Image joystickBg;
	[SerializeField]
	private Image joystick;

	private Vector2 inputVector;

	private void Start(){
		joystickBg = GetComponent<Image>();
		joystick = transform.GetChild(0).GetComponent<Image>();
	}
	public virtual void OnPointerDown(PointerEventData ped){
		OnDrag(ped);
	}
	public virtual void OnPointerUp(PointerEventData ped){
		inputVector = Vector2.zero;
		joystick.rectTransform.anchoredPosition = Vector2.zero;
	}
	public virtual void OnDrag(PointerEventData ped){
		Vector2 pos;
		if(RectTransformUtility.ScreenPointToLocalPointInRectangle(joystickBg.rectTransform,ped.position,ped.pressEventCamera,out pos)){
			pos.x=(pos.x/ joystickBg.rectTransform.sizeDelta.x);
			pos.y=(pos.y/ joystickBg.rectTransform.sizeDelta.x);

			inputVector =  new Vector2(pos.x *2 -1,pos.y*2-1);
			inputVector =(inputVector.magnitude >1.0f)? inputVector.normalized:inputVector;
			joystick.rectTransform.anchoredPosition = new Vector2(inputVector.x*(joystickBg.rectTransform.sizeDelta.x/2),inputVector.y*(joystickBg.rectTransform.sizeDelta.y/2));
		}
	}

	public float Horizontal(){
		if(inputVector.x !=0) return inputVector.x;
		else return Input.GetAxis("Horizontal");
	}
	public float Vertical(){
		if(inputVector.x !=0) return inputVector.y;
		else return Input.GetAxis("Vertical");
	}




}
```

<p>
</p>
<B>[3]</B>Карта с возможностью передвижения для настольных ролевых игр  "MapExplorer".
Основные реализованные механики:

1)Загрузка и смена карты в реальном временги прямо с файлов компютера.

2)Перемещение по карте.

3)Открытие тумана войны и его обновление.

https://disk.yandex.ru/d/bfuISVXkU4TZow

![map45](https://github.com/user-attachments/assets/1c3f5471-b482-4302-86ff-f3794fd355e7)
<p>Пример кода:</p>

```ruby
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AI;
using UnityEngine.UI;

using System;
using UnityEditor;

using System.Threading.Tasks;

using UnityEngine.Networking;
using SFB;


public class GlobalLogic : MonoBehaviour
{
    public GameObject playersTeam;
    public GameObject pointMarker;
    public GameObject fog;
    public NavMeshAgent agent;
    Vector3 pos;
    public Animator anim;
    bool animate = false;
    public GameObject[] buttons = new GameObject[2];
    public bool move = true;
    public Camera Cam;

    const float height = 0.4f;

    public string path;
    public Image image;

    [SerializeField] string _imageUrl;
    [SerializeField] Material _material;
    Texture2D _texture;

    public float timer = 0f;
    public float pointTimer = 0f;

    public static async Task<Texture2D> GetRemoteTexture(string url)
    {
        using (UnityWebRequest www = UnityWebRequestTexture.GetTexture(url))
        {
            // begin request:
            var asyncOp = www.SendWebRequest();

            // await until it's done: 
            while (asyncOp.isDone == false)
                await Task.Delay(1000 / 30);//30 hertz

            // read results:
            if (www.isNetworkError || www.isHttpError)
            // if( www.result!=UnityWebRequest.Result.Success )// for Unity >= 2020.1
            {
                // log error:
#if DEBUG
                Debug.Log($"{www.error}, URL:{www.url}");
#endif

                // nothing to return on error:
                return null;
            }
            else
            {
                // return valid results:
                return DownloadHandlerTexture.GetContent(www);

            }
        }
    }
    
    void Update()
    {
        if (pointTimer <= 0) {
            if (Input.GetMouseButtonDown(0))
            {
                pointTimer = 3;
                Ray movePosition = Camera.main.ScreenPointToRay(Input.mousePosition);
                if (Physics.Raycast(movePosition, out var hitInfo))
                {
                    print(hitInfo.point);
                    if (hitInfo.point.y <1)
                   {
                        pos = hitInfo.point;
                        pointMarker.transform.position = hitInfo.point + new Vector3(0, 0.5f, 0);
                    }
                    else
                    {
                        pos = hitInfo.point+new Vector3(0,0,-1.2f);
                        pointMarker.transform.position = hitInfo.point + new Vector3(0, 0.5f, 0);
                    }
                }
            }
        }
        else
        {
            pointTimer -= Time.deltaTime*2 ;
        }
        CamZoom();
        CamMove();
        if (timer <= 0)
        {
            HotKeys();
        }
        else
        {
            timer -= Time.deltaTime * 2;
        }
    }

    public void HotKeys()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            pauseApp();
        }
    }
    public void CamZoom()
    {
        Cam.fieldOfView -= Input.GetAxis("Mouse ScrollWheel") * 20;
        Cam.fieldOfView = Mathf.Clamp(Cam.fieldOfView, 10, 70);
    }
    public void CamMove()
    {
        if (Input.GetKey(KeyCode.W) || Input.GetKey(KeyCode.UpArrow))
        {
            Cam.transform.Translate(new Vector3(0,0.5f,0.5f)*Time.deltaTime*4);
        }
        if (Input.GetKey(KeyCode.S) || Input.GetKey(KeyCode.DownArrow))
        {
            Cam.transform.Translate(new Vector3(0, -0.5f, -0.5f) * Time.deltaTime*4);
        }

        if (Input.GetKey(KeyCode.D) || Input.GetKey(KeyCode.RightArrow))
        {
            Cam.transform.Translate(new Vector3(0.5f, 0, 0) * Time.deltaTime * 4);
        }
        if (Input.GetKey(KeyCode.A) || Input.GetKey(KeyCode.LeftArrow))
        {
            Cam.transform.Translate(new Vector3(-0.5f, 0, -0) * Time.deltaTime * 4);
        }
    }

    async public void OpenFile()
     {
    var extensions = new[] {  //какие файлы вообще можно открыть
            new ExtensionFilter("Image Files", "png", "jpg", "jpeg" ),
            new ExtensionFilter("All Files", "*" ),
        };
    foreach (string path in StandaloneFileBrowser.OpenFilePanel("Open File", "", extensions, true))
    { //открытие формы для загрузки файла
        Debug.Log(path);
            string path2 = "";
            for (int i = 0; i < path.Length; i++)
            {
                if ($"{path[i]}" != $"{path[2]}")
                {
                    path2 += $"{path[i]}";  
                }
                else
                {
                    path2 += "/";
                } 
            }
            _texture = await GetRemoteTexture(path2);
            _material.mainTexture = _texture;
        }
     }

    public void Go()
    {
        print("sd");
        agent.SetDestination(pos);
    }

    public void ApearMeny()
    {
        animate = !animate;
        anim.SetBool("Apear", animate);
    }
    public void AplicationExit()
    {
        Application.Quit();
    }

    public void pauseApp()
    {
        if (move)
        {
            buttons[0].SetActive(true);
            buttons[1].SetActive(false);
            agent.Stop();
        }
        else
        {
            buttons[0].SetActive(false);
            buttons[1].SetActive(true);
            agent.Resume();
        }
        move = !move;
        timer = 0.5f;
    }
    public void RefreshClouds()
    {
        for(int i = 0; i < 215; i++)
        {
            fog.transform.GetChild(i).gameObject.SetActive(true);
        }
    }
}
```



