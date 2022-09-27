# Horizon_2

Item_Enum
====
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Item : MonoBehaviour
{
  public enum ItemType
    {
        None,
        Wood,
        String,
        Coal,
        Metal_scrap,
        Gummy_bear,
        Rose_petal,
        Paper,
        Cotton,
        Glass,
        Playing_card
    }
    public ItemType Type;
    public int amount;

}

====

Item_ScriptableObject
====
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[CreateAssetMenu(menuName = "ScriptableObjects/ItemScriptableObject")]
public class ItemScriptableObject : ScriptableObject
{
    public Item.ItemType itemType;
    public string itemName;
    public Sprite itemSprite;

}
====
Item_Inventory
====
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Inventory {

    private List<Item> itemList;


    public Inventory()
    {
        itemList = new List<Item>();

        AddItem(new Item { Type = Item.ItemType.Wood , amount = 1});
        Debug.Log(itemList.Count);
    }
    public void AddItem(Item item)
    {
        itemList.Add(item);
    }

}

====
Item_UI
====
using System.Collections;
using System.Collections.Generic;
using Unity.Mathematics;
using UnityEngine;

public class Item_UI : MonoBehaviour
{
    private Inventory inventory;
    private Transform itemSlotContainer;
    private Transform itemSlotsTemplate;

    private void Awake()
    {
        itemSlotContainer = transform.Find("itemSlotsContainer");
        itemSlotsTemplate = itemSlotContainer.Find("itemSlotTemplate");
    }

    public void SetInventory(Inventory inventory)
    {
        this.inventory = inventory;
    }

    private void RefreshInventoryItems()
    {
        int x = 0;
        int y = 0;

        foreach (Item item in inventory.GetItemList())
        {
            RectTransform itemSlotRectTransform = Instantiate(itemSlotsTemplate, itemSlotContainer).GetComponent<RectTransform>();
            itemSlotRectTransform.gameObject.SetActive(true);
            itemSlotRectTransform.anchoredPosition = new Vector2(x * itemSlotCellSize, y * itemSlotCellSize);
            x++;

            if (x > 4)
            {
                x = 0;
                y++;
            }

        }
    }

}



