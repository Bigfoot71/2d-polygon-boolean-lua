# 2d-polygon-boolean

Port Lua de l'implementation Javascript de l'algorihm de Greiner-Hormann "[efficient clipping of arbitrary polygons](http://www.inf.usi.ch/hormann/papers/Greiner.1998.ECO.pdf)" réalisé par [tmpvar](https://github.com/tmpvar). Lien du repository d'origine [ici](https://github.com/tmpvar/2d-polygon-boolean).

## Use

Ce module s'importe comme ceci:

```lua
local polygonBoolean = require("2d-polygon-boolean")
```

Les polygons donnés en paramètre doivent être représentés par des tables construites ainsi:

```lua
polygon = { x1,y1, x2,y2, x3,y3, ... }
```

Et la fonction doit être appelé comme ceci:

```lua
polygons = polygonBoolean(polygon_subject, polygon_clip, operation, get_most_revelant)
```

`polygon_subject` est le polygon sujet de l'operation.
`polygon_clip` est le polygon qui fera subir l'operation.

`operation` est une valeur chaine de caractère qui peut être:
>`or` : pour effectuer une union des polygones.
`not` : pour effectuer une exclusion de la partie chevauchante sur le polygone sujet.
`and` : pour ne garder que que la partie chevauchante.
>

`get_most_revelant` sert à ne garder que le resultat le plus pertinent. Par defaut ce paramètre est sur faux et donc la fonction retourne une table de plusieurs polygones (qu'une intersection a été trouvé ou non) comme ceci:
```lua
{
    { x1,y1, x2,y2, x3,y3, ... },
    { x1,y1, x2,y2, x3,y3, ... },
    ...
}
```
Si ce paramètre est vrai et qu'une intersection a été trouvé il renverra le polygones resultant contenant le plus de sommets (est pertinent dans la plupars des cas mais peut ne pas l'être dans certains rare cas comme avec l'exclusion) ou renvoie `false, polygones` dans le cas où aucune intersection n'a été trouvé entre le polygone sujet et le polygon clip.

### Exemple complet

```lua

local polygonBoolean = require("2d-polygon-boolean")

local subject = { 0,0, 100,0, 100,100, 0,100 }
local clip = { 90,90, 110,90, 110,110, 90,110, 90,90 }

local union = polygonBoolean(subject, clip, "or")

for i, result in ipairs(union) do
    print("result "..i..": ")
    for j = 1, #result-1, 2 do
        print(
            "x"..((j+1)/2)..": "..result[j],
            "y"..((j+1)/2)..": "..result[j+1]
        )
    end
end

--[[
result 1: 
    x1: 100 y1: 90
    x2: 110 y2: 90
    x3: 110 y3: 110
    x4: 90  y4: 110
    x5: 90  y5: 100
    x6: 0   y6: 100
    x7: 0   y7: 0
    x8: 100 y8: 0
]]

local cut = polygonBoolean(subject, clip, "not")

for i, result in ipairs(cut) do
    print("result "..i..": ")
    for j = 1, #result-1, 2 do
        print(
            "x"..((j+1)/2)..": "..result[j],
            "y"..((j+1)/2)..": "..result[j+1]
        )
    end
end

--[[
result 1: 
    x1: 100 y1: 90
    x2: 90  y2: 90
    x3: 90  y3: 100
    x4: 0   y4: 100
    x5: 0   y5: 0
    x6: 100 y6: 0
]]

local intersect = polygonBoolean(subject, clip, "and")

for i, result in ipairs(intersect) do
    print("result "..i..": ")
    for j = 1, #result-1, 2 do
        print(
            "x"..((j+1)/2)..": "..result[j],
            "y"..((j+1)/2)..": "..result[j+1]
        )
    end
end

--[[
result 1: 
    x1: 100 y1: 90
    x2: 90  y2: 90
    x3: 90  y3: 100
    x4: 100 y4: 100
]]
```

# License

[MIT](LICENSE)
