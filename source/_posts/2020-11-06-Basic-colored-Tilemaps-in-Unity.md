---
title: Basic colored Tilemaps in Unity
date: 2020-11-06 16:32:29
tags:
- unity
- programming
- c#
---

Unity comes with built-in [Grid](https://docs.unity3d.com/2019.3/Documentation/ScriptReference/Grid.html) and [Tilemap](https://docs.unity3d.com/Manual/class-Tilemap.html) class which makes it super easy to build 2D games that use rectangular, hexagonal or isometric layouts. Here is how you can use this to generate plain colours in place of each tile imperatively rather than through the GUI.

<!-- more -->

The general process is as follows:
* Set up a material applied to the tile map, anything works
* Create a texture matching the grid size - I chose 16 so I have flexibility to later add cute textures, but it doesn’t matter
* Generate a sprite from said texture and apply it to a tile
* Add the tile(s)

## Generating a texture

This is fairly straightforward, just creating an empty texture and setting all the pixel colours. Don’t forget to `Apply()`.
```C#
// Generate the texture
Color grassColor = Color.green; // you could parameterize this of course
Texture2D grassTex = new Texture2D(16, 16);
for (int x = 0; x < 16; x++) {
	for (int y = 0; y < 16; y++) {
		grassTex.setPixel(x, y, grassColor);
	}
}
grassTex.Apply();
```

## Generating a single tile

Tiles need /sprites/ not textures added to them. So first we need to create a sprite and apply the above texture:

```C#
Sprite grassSprite = Sprite.Create(grassTex, new Rect(0, 0, 16, 16), new Vector2(0.5f, 0.5f, 16f);
Tile grassTile = ScriptableObject.CreateInstance<Tile>();
grassTile.sprite = grassSprite;
```

Of course the above 2 steps you can repeat for any other placeholder tiles you want.

## Applying the Tile to the Tilemap

Add this tile as you wish using the usual `SetTile()` function, something like this:

```C#
int mapsize = 100;
for (int x = 0; x < mapsize; x++) {
	for (int x = 0; x < mapsize; x++) {
		tilemap.SetTile(new Vector3Int(x, y, 0), grassTile);
	}
}
```

Next I’ll be looking at the classic Perlin noise approach to generating some more interesting dynamic tile terrain using this technique :)
