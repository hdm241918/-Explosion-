

 using System.Collections;
 using System.Collections.Generic;
using UnityEngine;

public class Boom : MonoBehaviour
{
   public float radius = 10f;   //定义一个要添加爆炸力的半径
   public float power = 600f;   //定义一个爆炸力
   public GameObject particle;   //得到播放粒子特效的物体

    private void Start()
    {
        
    }
    // Update is called once per frame
    void Update()
    {
        //当左键按下时
        if (Input.GetMouseButtonDown(0))
        {
            //Camera.main：得到主摄像机
            //Input.mousePosition：得到鼠标现在的位置
            //我的理解是从主摄像机到鼠标现在的点发射一条射线
            Ray ray;
              

            //光线投射碰撞
            RaycastHit hit;
             ray = Camera.main.ScreenPointToRay(Input.mousePosition);
            //函数是对射线碰撞的检测，当碰撞到物体时，返回一个碰撞信息
            if (Physics.Raycast(ray, out hit))
            {
                Debug.Log(1);
                Vector3 point = hit.point;//得到碰撞点的坐标

                //实例化出这个物体
                Instantiate(particle, point, Quaternion.identity);

                //Physics.OverlapSphere（）：球体投射，给定一个球心和半径，返回球体投射到的物体的碰撞器
                Collider[] colliders = Physics.OverlapSphere(point, radius);
                foreach (Collider hits in colliders)  //遍历碰撞器数组
                {
                    //如果这个物体有刚体组件
                    if (hits.GetComponent<Rigidbody>())
                    {
                        //给定爆炸力大小，爆炸点，爆炸半径
                        //利用刚体组件添加爆炸力AddExplosionForce
                        hits.GetComponent<Rigidbody>().AddExplosionForce(power, point, radius);
                    }
                }
            }
        }
    }
}   
