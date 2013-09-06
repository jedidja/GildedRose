# Gilded Rose Refactoring Kata


## Introduction
Hi team, and welcome! As you know, we are a small inn (with a prime location in a prominent city) run by a friendly innkeeper named Allison. We also buy and sell only the finest goods. 

Unfortunately, the goods are constantly degrading in quality as they approach their sell-by date. We have a system in place that updates our inventory for us. It was developed by a no-nonsense type named Leeroy, who has moved on to new adventures. 

Your task is to add the new feature to our system so that we can begin selling a new category of items.

## Current System Behvaior

- All items have a `SellIn` value which denotes the number of days we have to sell the item
- All items have a `Quality` value which denotes how valuable the item is
- At the end of each day our system lowers both values for every item

Pretty simple, right? Well this is where it gets interesting:

- Once the sell by date has passed, `Quality` degrades twice as fast
- The `Quality` of an item is never negative
- "Aged Brie" actually increases in `Quality` the older it gets
- The `Quality` of an item is never more than 50
- "Sulfuras", being a legendary item, never has to be sold or decreases in Quality
- "Backstage passes", like "Aged Brie", increases in Quality as its `SellIn` value approaches; `Quality` increases by 2 when there are 10 days or less and by 3 when there are 5 days or less but `Quality` drops to 0 after the concert

## New Requirements
We have recently signed a supplier of conjured items. This requires an update to our system:

- "Conjured" items degrade in Quality twice as fast as normal items

## Restrictions
Feel free to make any changes to the `UpdateQuality` method and add any new code as long as everything still works correctly. However, do not alter the `Item` class or `Items` property as those belong to the goblin in the corner who will insta-rage and one-shot you as he doesn't believe in shared code ownership (you can make the `UpdateQuality` method and `Items` property static if you like, we'll cover for you).

Just for clarification, an item can never have its `Quality` increase above 50, however "Sulfuras" is a legendary item and as such its Quality is 80 and it never alters.

```csharp
class Item
{
    public string Name { get; set; }

    public int SellIn { get; set; }

    public int Quality { get; set; }
}


IList<Item> Items = new List<Item>
{
	new Item { Name = "+5 Dexterity Vest", SellIn = 10, Quality = 20 },
	new Item { Name = "Aged Brie", SellIn = 2, Quality = 0 },
	new Item { Name = "Elixir of the Mongoose", SellIn = 5, Quality = 7 },
	new Item { Name = "Sulfuras, Hand of Ragnaros", SellIn = 0, Quality = 80 },
	new Item { Name = "Backstage passes to a Justin Bieber concert", SellIn = 15, Quality = 20 },
	new Item { Name = "Conjured Mana Cake", SellIn = 3, Quality = 6 }
};

void UpdateQuality()
{
    for (var i = 0; i < Items.Count; i++)
    {
        if (Items[i].Name != "Aged Brie" && 
            Items[i].Name != "Backstage passes to a Justin Beiber concert")
        {
            if (Items[i].Quality > 0)
            {
                if (Items[i].Name != "Sulfuras, Hand of Ragnaros")
                {
                    Items[i].Quality = Items[i].Quality - 1;
                }
            }
        }
        else
        {
            if (Items[i].Quality < 50)
            {
                Items[i].Quality = Items[i].Quality + 1;

                if (Items[i].Name == "Backstage passes to a Justin Bieber concert")
                {
                    if (Items[i].SellIn < 11)
                    {
                        if (Items[i].Quality < 50)
                        {
                            Items[i].Quality = Items[i].Quality + 1;
                        }
                    }

                    if (Items[i].SellIn < 6)
                    {
                        if (Items[i].Quality < 50)
                        {
                            Items[i].Quality = Items[i].Quality + 1;
                        }
                    }
                }
            }
        }

        if (Items[i].Name != "Sulfuras, Hand of Ragnaros")
        {
            Items[i].SellIn = Items[i].SellIn - 1;
        }

        if (Items[i].SellIn < 0)
        {
            if (Items[i].Name != "Aged Brie")
            {
                if (Items[i].Name != "Backstage passes to a Justin Bieber concert")
                {
                    if (Items[i].Quality > 0)
                    {
                        if (Items[i].Name != "Sulfuras, Hand of Ragnaros")
                        {
                            Items[i].Quality = Items[i].Quality - 1;
                        }
                    }
                }
                else
               {
                    Items[i].Quality = Items[i].Quality - Items[i].Quality;
                }
            }
            else
            {
                if (Items[i].Quality < 50)
               {
                    Items[i].Quality = Items[i].Quality + 1;
                }
            }
        }
    }
}
```
