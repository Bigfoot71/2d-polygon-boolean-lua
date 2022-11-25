
# 2d-polygon-boolean-lua

Port Lua of the Javascript implementation of the Greiner-Hormann algorithm taken from the paper "[efficient clipping of arbitrary polygons](http://www.inf.usi.ch/hormann/papers/Greiner.1998.ECO.pdf)" made by [tmpvar](https://github.com/tmpvar). Original repository link [here](https://github.com/tmpvar/2d-polygon-boolean).

## Use

This module is imported like this:

```lua
local polygonBoolean = require("2d-polygon-boolean")
```

The polygons given as a parameter must be represented by tables constructed as follows:

```lua
polygon = { x1,y1, x2,y2, x3,y3, ... }
```

And the function should be called like this:

```lua
polygons = polygonBoolean(polygon_subject, polygon_clip, operation, get_most_revelant)
```

`polygon_subject` is the subject polygon of the operation.
`polygon_clip` is the polygon that will perform the operation.

`operation` is a string value which can be:
>`or` : to perform a union of the polygons.
>`not` : to perform an overlapping exclusion on the subject polygon.
>`and` : to keep only the overlapping part.

`get_most_revelant` is used to keep only the most relevant result. By default this parameter is false and therefore the function returns a table of several polygons (whether an intersection was found or not) like this:
```lua
{
    { x1,y1, x2,y2, x3,y3, ... },
    { x1,y1, x2,y2, x3,y3, ... },
    ...
}
```
If this parameter is true and an intersection was found it will return the resulting polygon containing the most vertices (is relevant in most cases but may not be in some rare cases like with exclusion) or returns `false, polygons` in case no intersection was found between the subject polygon and the clip polygon.

### Complete example

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

[MIT](LICENSE)# 2d-polygon-boolean-lua

Port Lua of the Javascript implementation of the Greiner-Hormann algorithm taken from the paper "[efficient clipping of arbitrary polygons](http://www.inf.usi.ch/hormann/papers/Greiner.1998.ECO.pdf)" made by [tmpvar](https://github.com/tmpvar). Original repository link [here](https://github.com/tmpvar/2d-polygon-boolean).

## Use

This module is imported like this:

```lua
local polygonBoolean = require("2d-polygon-boolean")
```

The polygons given as a parameter must be represented by tables constructed as follows:

```lua
polygon = { x1,y1, x2,y2, x3,y3, ... }
```

And the function should be called like this:

```lua
polygons = polygonBoolean(polygon_subject, polygon_clip, operation, get_most_revelant)
```

`polygon_subject` is the subject polygon of the operation.
`polygon_clip` is the polygon that will perform the operation.

`operation` is a string value which can be:
>`or` : to perform a union of the polygons.
>`not` : to perform an overlapping exclusion on the subject polygon.
>`and` : to keep only the overlapping part.

`get_most_revelant` is used to keep only the most relevant result. By default this parameter is false and therefore the function returns a table of several polygons (whether an intersection was found or not) like this:
```lua
{
    { x1,y1, x2,y2, x3,y3, ... },
    { x1,y1, x2,y2, x3,y3, ... },
    ...
}
```
If this parameter is true and an intersection was found it will return the resulting polygon containing the most vertices (is relevant in most cases but may not be in some rare cases like with exclusion) or returns `false, polygons` in case no intersection was found between the subject polygon and the clip polygon.

### Complete example

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

