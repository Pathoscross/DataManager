# DataManager

DataManager 是一个简易的本地资料库工具，依据企划内容来自动化产出相对应的程式码

## 支援资料型态
* bool
* float
* int
* string
* List\<string\>

## Excel 资料范例
### Player.csv
|string/Key|int/Level|int/Hp|int/Exp|
|:-------------:|:-------------:|:-------------:|:-------------:|
|0	|1	|10	|10|
|1	|2	|20	|20|
|2	|3	|30	|30|
|3	|4	|40	|40|
|4	|5	|50	|50|
|5	|6	|60	|60|
|6	|7	|70	|70|
|7	|8	|80	|80|
|8	|9	|90	|90|
|9	|10	|100	|100|

转换后类别
```C#
public class PlayerData
{
	public string Key;
	public int Level;
	public int Hp;
	public int Exp;
}
```
### Monster.csv
|string/Key|string/Name|int/Hp|
|:-------------:|:-------------:|:-------------:|
|0	|Monster_1	|10	|
|1	|Monster_2	|20	|
|2	|Monster_3	|30	|
|3	|Monster_4	|40	|
|4	|Monster_5	|50	|
|5	|Monster_6	|60	|
|6	|Monster_7	|70	|
|7	|Monster_8	|80	|
|8	|Monster_9	|90	|
|9	|Monster_10	|100	|

转换后类别
```C#
public class MonsterData
{
	public string Key;
	public string Name;
	public int Hp;
}
```

## 自动化脚本生成流程
1. 使用 Excel 工具编辑资料表内容
2. 将资料表汇出成 .csv 格式
3. 将 .csv 档案放入 DataTool/Resources/CsvResources/ 目录下
4. 点选 TEDTool/Data/Generate Script

## 使用范例
```C#
using UnityEngine;
using System.Collections;
using TEDTool.Data;

public class Example : MonoBehaviour
{
	private DataManager m_dataManager;

	void Awake()
	{
		m_dataManager = new DataManager();
		m_dataManager.Load();

		PrintPlayerData();
		PrintMonsterData();
	}


	private void PrintPlayerData()
	{
		PlayerDataType playerDataType = m_dataManager.GetDataType<PlayerDataType>();
		PlayerData playerData = null;
		
		for(int cnt = 0; cnt < playerDataType.GetCount(); cnt++)
		{
			playerData = playerDataType.GetData(cnt.ToString());

			Debug.Log(string.Format("PlayerData_{0} : Key = {1}, Level = {2}, Hp = {3}, Exp = {4}",
			                        cnt, playerData.Key, playerData.Level, playerData.Hp, playerData.Exp));
		}
	}


	private void PrintMonsterData()
	{
		MonsterDataType monsterDataType = m_dataManager.GetDataType<MonsterDataType>();
		MonsterData monsterData = null;
		
		for(int cnt = 0; cnt < monsterDataType.GetCount(); cnt++)
		{
			monsterData = monsterDataType.GetData(cnt.ToString());

			Debug.Log(string.Format("MonsterData_{0} : Key = {1}, Name = {2}, Hp = {3}",
			                        cnt, monsterData.Key, monsterData.Name, monsterData.Hp));
		}
	}
}
```