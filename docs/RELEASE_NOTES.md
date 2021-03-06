# cocos2d-x v3.0 Release Notes #

**Table of Contents**  *generated with [DocToc](http://doctoc.herokuapp.com/)*

- [cocos2d-x v3.0 Release Notes](#cocos2d-x-v30-release-notes)
- [Misc Information](#misc-information)
- [Requirements](#requirements)
	- [Runtime Requirements](#runtime-requirements)
	- [Compiler Requirements](#compiler-requirements)
- [Highlights of v3.0](#highlights-of-v30)
- [Features in detail](#features-in-detail)
	- [C++11 features](#c11-features)
		- [std::function](#stdfunction)
		- [strongly typed enums](#strongly-typed-enums)
		- [override](#override)
	- [Removed Objective-C patterns](#removed-objective-c-patterns)
		- [No more 'CC' prefix for C++ classes and free functions](#no-more-cc-prefix-for-c-classes-and-free-functions)
		- [clone() instead of copy()](#clone-instead-of-copy)
		- [Singletons use getInstance() and destroyInstance()](#singletons-use-getinstance-and-destroyinstance)
		- [Object is replaced with Ref](#object-is-replaced-with-ref)
		- [getters](#getters)
		- [POD types](#pod-types)
	- [New Renderer](#new-renderer)
		- [Renderer features](#renderer-features)
			- [Auto-batching](#auto-batching)
			- [Auto-culling](#auto-culling)
			- [Global Z order](#global-z-order)
	- [Improved LabelTTF / LabelBMFont / LabelAtlas](#improved-labelttf--labelbmfont--labelatlas)
	- [New EventDispatcher](#new-eventdispatcher)
	- [Physics Integration](#physics-integration)
		- [PhysicsWorld](#physicsworld)
		- [PhysicsBody](#physicsbody)
		- [PhysicsShape](#physicsshape)
		- [PhysicsJoint](#physicsjoint)
		- [PhysicsContact](#physicscontact)
- [Misc API Changes](#misc-api-changes)
	- [ccTypes.h](#cctypesh)
	- [deprecated functions and  global variables](#deprecated-functions-and--global-variables)
- [Changes in the Lua bindings](#changes-in-the-lua-bindings)
    - [Use bindings-generator tool for lua binding](#use-bindings-generator-tool-for-lua-binding)
	    - [Bind the classes with namespace to lua](#bind-the-classes-with-namespace-to-lua)
	    - [Use ScriptHandlerMgr to manage the register and unregister of Lua function](#use-scripthandlermgr-to-manage-the-register-and-unregister-of-lua-function)
	- [Misc API changes](#misc-api-changes-1)
		- [Use cc、ccs、ccui、gl and sp as module name](#use-ccccsccuigl-and-sp-as-module-name)
		- [Modified functions](#modified-functions)
		- [Add some modules](#add-some-modules)
		- [Add more lua bindings](#add-more-lua-bindings)
		- [Replace some lua-bindings of Class or Struct with lua table](#replace-the-lua-bindings-of-class-or-struct-with-lua-table)
	- [Other Changes](#other-changes)
	    - [Support lua script codes call Obeject-C codes and Java codes](#support-lua-script-codes-call-OC-codes-and-Java-codes)
	    - [Add some lua files to store the constants of different modules](#add-some-lua-files-to-store-the-constants-of-different-modules)			

# Misc Information

* Download: http://cdn.cocos2d-x.org/cocos2d-x-3.0beta.zip
* Full Changelog: https://github.com/cocos2d/cocos2d-x/blob/cocos2d-x-3.0beta/CHANGELOG
* API Reference: http://www.cocos2d-x.org/reference/native-cpp/V3.0beta/index.html

# Requirements

## Runtime Requirements

* Android 2.3 or newer
* iOS 5.0 or newer
* OS X 10.7 or newer
* Windows 7 or newer
* ~~Windows Phone 8 or newer~~ N/A for the moment
* Linux Ubuntu 12.04 (or newer)
* ~~Browsers via Emscripten~~ N/A for the moment
* ~~Marmalade~~ N/A for the moment
* ~~BlackBerry~~ N/A for the moment

## Compiler Requirements

* Xcode 4.6 (for iOS or Mac)
* gcc 4.7 for Linux or Android. For Android ndk-r9 or newer is required.
* Visual Studio 2012 (for Windows)

## Run samples

### Mac OSX & iOS

* Enter `cocos2d-x/build` folder, open `cocos2d_test.xcodeproj`
* Select `iOS` or `OS X` target in scheme toolbar
* Click `run` button

### Android

Can run sample on Android in two ways:

* Use command to run
* Use Eclipse to run

**By commnad**

    $ cd cocos2d-x
    $ ./install.py
    $ cd build
    $ ./android-build.py -p 10
    $ adb install cocos2d-x/tests/proj.android/bin/TestDemo-debug.apk
    
Then click item on Android device to run tests. Available value of `-p` is the API level, cocos2d-x supports from level 10.

**Using Eclipse**

    $ cd cocos2d-x
    $ ./install.py
    $ cd build
    $ ./android-build.py
    
Then

* Import cocos2d-x Android project into Eclipse, the path used to import is `cocos/2d/platform/android`
* Import `tests` Android project into Eclipse, the path used to import is `tests/proj.android`
* Build `tests` Android project and run

### Windows

* Enter `cocos2d-x/build`, and open `cocos2d-win32.vs2012.sln`
* Select `TestCpp` as running target
* Click run button


### Linux

    $ cd cocos2d-x/build
    $ ./install-deps-linux.sh
    $ cd ../..
    
Then

    $ mkdir build
    $ cd build
    $ cmake ..
    $ make -j4
    
Run

    $ cd bin/testcpp
    $ ./testcpp
    

# Highlights of v3.0

* Replaced Objective-C patters with C++ (C++11) patterns and best practices
* Improved Labels
* Improved renderer
* New Event Dispatcher
* Physics integration
* New GUI
* [JavaScript remote debugger](https://github.com/cocos2d/cocos-docs/blob/master/manual/framework/native/scripting/javascript/js-remote-debugger/en.md)
* Remote Console support
* Refactor Image - release memory in time and uniform the api of supported file format
* Automatically generated Lua bindings, add LuaJavaBridge and LuaObjcBridge
* Templated containers
    * `CCDictionary` is replaced by `cocos2d::Map<>`, [usage](https://github.com/cocos2d/cocos-docs/blob/master/manual/framework/native/data-structure/v3/map/en.md)
    * `CCArray` is replaced by `cocos2d::Vector<>`, [usage](https://github.com/cocos2d/cocos-docs/blob/master/manual/framework/native/data-structure/v3/vector/en.md)
    * `CCBool`, `CCFLoat`, `CCDouble` are replaced with `cocos2d::Value`, [usage](https://github.com/cocos2d/cocos-docs/blob/master/manual/framework/native/data-structure/v3/value/en.md)

# Features in detail

## C++11 features

_Feature added in v3.0-pre-alpha0_

A subset of C++11 features are being used in cocos2d-x:

* `std::function`, including lambda objects for callbacks
* strongly typed enums, for most of the cocos2d-x enums and constants
* `std::thread` for threading
* `override` context keyword, for overriden methods


### std::function

* `CallFunc` can be created with an `std::function<void()>`
* `CallFuncN` can be created with an `std::function<void(Node*)>`
* `CallFuncND` and `CallFuncO` were removed since it can be created with simulated with `CallFuncN` and `CallFunc`. See ActionsTest.cpp for more examples
* `MenuItem` supports `std::function<void(Node*)>` as callbacks

`CallFunc` example:

```cpp
// in v2.1
CCCallFunc *action1 = CCCallFunc::create( this, callfunc_selector( MyClass::callback_0 ) );

// in v3.0 (short version)
auto action1 = CallFunc::create( CC_CALLBACK_0(MyClass::callback_0,this));
auto action2 = CallFunc::create( CC_CALLBACK_0(MyClass::callback_1,this, additional_parameters));

// in v3.0 (long version)
auto action1 = CallFunc::create( std::bind( &MyClass::callback_0, this));
auto action2 = CallFunc::create( std::bind( &MyClass::callback_1, this, additional_parameters));

// in v3.0 you can also use lambdas or any other "Function" object
auto action1 = CallFunc::create(
                 [&](){
                     auto s = Director::sharedDirector()->getWinSize();
                     auto label = LabelTTF::create("called:lambda callback", "Marker Felt", 16);
                     label->setPosition(ccp( s.width/4*1,s.height/2-40));
                     this->addChild(label);
                 }  );
```

`MenuItem` example:
```c++
// in v2.1
CCMenuItemLabel *item = CCMenuItemLabel::create(label, this, menu_selector(MyClass::callback));

// in v3.0 (short version)
auto item = MenuItemLabel::create(label, CC_CALLBACK_1(MyClass::callback, this));

// in v3.0 (long version)
auto item = MenuItemLabel::create(label, std::bind(&MyClass::callback, this, std::placeholders::_1));

// in v3.0 you can use lambdas or any other "Function" object
auto item = MenuItemLabel::create(label,
                 [&](Object *sender) {
                     // do something. Item "sender" clicked
                  });
```

### strongly typed enums

_Feature added in v3.0-pre-alpha0_

Constants and enums that started with `k`, and that usually were defined as `int` or as simple `enum` where replaced with strongly typed enums ( `enum class` ) to prevent collisions and type errors.
The new format is:

    | v2.1       | v3.0        |
    | kTypeValue | Type::VALUE |

Examples:

	| v2.1                             | v3.0                             |
	| kCCTexture2DPixelFormat_RGBA8888 | Texture2D::PixelFormat::RGBA8888 |
	| kCCDirectorProjectionCustom      | Director::Projection::CUSTOM     |
	| ccGREEN                          | Color3B::GREEN                   |
	| CCPointZero                      | Point::ZERO                      |
	| CCSizeZero                       | Size::ZERO                       |

The old values can still be used, but are deprecated.

### override

To catch possible errors while overriding methods, subclasses with override methods have the `override` context keyword.
Example:
```c++
class Sprite : public Node {
    bool isFlipY(void) const;
    void setFlipY(bool bFlipY);

    // Overrides
    virtual void setTexture(Texture2D *texture) override;
    virtual Texture2D* getTexture() const override;
    inline void setBlendFunc(const BlendFunc &blendFunc) override;
    inline const BlendFunc& getBlendFunc() const override;
}
```

## Removed Objective-C patterns

_Feature added in v3.0-pre-alpha0_

### No more 'CC' prefix for C++ classes and free functions

*Changes in classes*

Since cocos2d-x already uses the `cocos2d` namespace, there is not need to add the prefix `CC` to all its classes.

Examples:

	| v2.1       | v3.0     |
	| CCSprite   | Sprite   |
	| CCNode     | Node     |
	| CCDirector | Director |
	| etc...                |

v2.1 class names are still available, but they were tagged as deprecated.

*Changes in free functions*

For the *drawing primitives*:
* They were added in the `DrawPrimitives` namespace
* The `cc` prefix was removed

For the *gl proxy functions*:
* They were added in the `GL` namespace
* The `ccGL` prefix was removed

Examples:

	| v2.1                | v3.0                         |
	| ccDrawPoint()       | DrawPrimitives::drawPoint()  |
	| ccDrawCircle()      | DrawPrimitives::drawCircle() |
	| ccGLBlendFunc()     | GL::blendFunc()              |
	| ccGLBindTexture2D() | GL::bindTexture2D()          |
	| etc...                       				          |

v2.1 free functions are still available, but they were tagged as deprecated.


### clone() instead of copy()

`clone()` returns an autoreleased version of the copy.

`copy()` is no longer supported. If you use it, it will compile, but the code will crash.

Example:
```c++
// v2.1
CCMoveBy *action = (CCMoveBy*) move->copy();
action->autorelease();

// v3.0
// No need to do autorelease, no need to do casting.
auto action = move->clone();
```

### Singletons use getInstance() and destroyInstance()

All singletons use `getInstance()` and `destroyInstance()` (if applicable) to get and destroy the instance.

Examples:

	| v2.1                          | v3.0                        |
	| CCDirector->sharedDirector()  | Director->getInstance()     |
	| CCDirector->endDirector()     | Director->destroyInstance() |
	| etc...                                                      |


v2.1 methods are still available, but they were tagged as deprecated.

### Object is replaced with Ref

Because the name `Object` is confused, so rename it to `Ref`, and remove functions that are not related with referenct count. All classes that inherit from `Object` now inherit from `Ref`.

### getters

Getters now use the `get` prefix.

Examples:

	| v2.1                            | v3.0* 				 		       |
	| node->boundingBox()             | node->getBoundingBox() 			   |
	| sprite->nodeToParentTransform() | sprite->getNodeToParentTransform() |
	| etc...                                                               |

And getters were also tagged as `const` in their declaration. Example:

```c++
// v2.1
virtual float getScale();

// v3.0
virtual float getScale() const;
```

v2.1 methods are still available, but they were tagged as deprecated.

### POD types

Methods that were receiving POD types as arguments (eg: `TexParams`, `Point`, `Size`, etc.) are being passed as `const` reference.

Example:
```c++
// v2.1
void setTexParameters(ccTexParams* texParams);

// v3.0
void setTexParameters(const ccTexParams& texParams);
```


## New Renderer

_Feature added in v3.0-beta and improved in v3.0-beta2_

The renderer functionality has been decoupled from the Scene graph / Node logic. A new object called `Renderer` is responsible for rendering the object.

Auto-batching and auto-culling support have been added.

Please, see this document for detail information about its internal funcitonality: [Renderer Specification document](https://docs.google.com/document/d/17zjC55vbP_PYTftTZEuvqXuMb9PbYNxRFu0EGTULPK8/edit)

### Renderer features

#### Auto-batching

TODO

#### Auto-culling

With auto-culling, sprites that outside screen will be thrown away.

#### Global Z order

A new method called `setGlobalZOrder()` / `getGlobalZOrder()` was added to `Node`, and the old  methods `setZOrder()` / `getZOrder()` were renamed to `setLocalZOrder()` / `getLocalZOrder()`.

`globalZOrder` receives a `float` (and not an `int`) as argument. And this value is used to sort the Nodes in the Renderer. Lower values have higher priority over higher values. That means that a Node with a `globalZOrder` of `-10` is going to be drawn BEFORE a Node with `globalZOrder` of `10`.

Nodes that have a `globalZOrder` of `0` (default value) will be drawn according to the Scene Graph order.

So, if the `globalZOrder` is not changed, cocos2d-x v3.0 will behave exaclty as cocos2d-x v2.2. 

__`globalZOrder()` vs. `localZOrder()`__:

* `globalZOrder` is used to sort the "draw commands" in the Renderer
* `localZOrder` is used to sort the Node in its parent's children Array

__Exceptions__:

TODO


## Improved LabelTTF / LabelBMFont / LabelAtlas

_Feature added in v3.0-alpha0_

`LabelTTF`, `LabelBMFont` and `LabelAtlas` will be replaced by new `Label`. The benifits of new `Label` are:

* uniform api to create `LabelTTF`, `LabelBMFont` and `LabelAtlas`
* use `freetype` to generate texture for labels, which make sure that labels have the same effect on different platforms
* will cache textures to improve performance

## New EventDispatcher

_Feature added in v3.0-alpha0_

All events such as touch event, keyboard event, acceleration event and custom event are dispatched by `EventDispatcher`.
`TouchDispatcher`, `KeypadDispatcher`, `KeyboardDispatcher`, `AccelerometerDispatcher` were removed.

Features of `EventDispatcher` are:

* dispatch events based on rendering sequence
* all events are dispatched by `EventDispatcher`
* can use `EventDispatcher` to dispatch custom events
* can register a lambda as call back function

Detail information of `EventDispatcher` can refer to [this document](https://github.com/cocos2d/cocos-docs/blob/master/manual/framework/native/input/event-dispatcher/en.md).

## Physics Integration

_Feature added in v3.0-pre-alpha0_

In v3.0, we integrate physics engien into cocos2d-x based on [Chipmunk2D](https://chipmunk-physics.net/). By using this feature, you can create physics based games without understanding physics engine.

More detail information of this feature, please refer to [this document](https://github.com/cocos2d/cocos-docs/blob/master/manual/framework/native/physics/physics-integration/en.md)


# Misc API Changes

## `ccTypes.h`

Remove *cc* prefix for structure names in ccTypes.h, move global functions into static member functions, and move global constants into const static member variables.

	| v2.1 struct names       | v3.0 struct names |
	| ccColor3B 	          | Color3B |
	| ccColor4B 		      | Color4B |
	| ccColor4F 		      | Color4F |
	| ccVertex2F 		      | Vertex2F |
	| ccVertex3F 		      | Vertex3F |
	| ccTex2F 			      | Tex2F |
	| ccPointSprite 	      | PointSprite |
	| ccQuad2 			      | Quad2 |
	| ccQuad3 			      | Quad3 |
	| ccV2F_C4B_T2F 	      | V2F_C4B_T2F |
	| ccV2F_C4F_T2F 	      | V2F_C4F_T2F |
	| ccV3F_C4B_T2F 	      | V3F_C4B_T2F |
	| ccV2F_C4B_T2F_Triangle  | V2F_C4B_T2F_Triangle |
	| ccV2F_C4B_T2F_Quad      | V2F_C4B_T2F_Quad |
	| ccV3F_C4B_T2F_Quad      | V3F_C4B_T2F_Quad |
	| ccV2F_C4F_T2F_Quad      | V2F_C4F_T2F_Quad |
	| ccBlendFunc 		      | BlendFunc |
	| ccT2F_Quad 		      | T2F_Quad |
	| ccAnimationFrameData    | AnimationFrameData |

Global functions changed example
```c++
// in v2.1
ccColor3B color3B = ccc3(0, 0, 0);
ccc3BEqual(color3B, ccc3(1, 1, 1));
ccColor4B color4B = ccc4(0, 0, 0, 0);
ccColor4F color4F = ccc4f(0, 0, 0, 0);
color4F = ccc4FFromccc3B(color3B);
color4F = ccc4FFromccc4B(color4B);
ccc4FEqual(color4F, ccc4F(1, 1, 1, 1));
color4B = ccc4BFromccc4F(color4F);

color3B = ccWHITE;

// in v3.0
Color3B color3B = Color3B(0, 0, 0);
color3B.equals(Color3B(1, 1, 1));
Color4B color4B = Color4B(0, 0, 0, 0);
Color4F color4F = Color4F(0, 0, 0, 0);
color4F = Color4F(color3B);
color4F = Color4F(color4B);
color4F.equals(Color4F(1, 1, 1, 1));
color4B = Color4B(color4F);

color3B = Color3B::WHITE;
```

## deprecated functions and  global variables

	| v2.1 names 	| v3.0 names |
	| ccp 			| Point |
	| ccpNeg 		| Point::- |
	| ccpAdd 		| Point::+ |
	| ccpSub 		| Point::- |
	| ccpMult 		| Point::* |
	| ccpMidpoint 	| Point::getMidpoint |
	| ccpDot 		| Point::dot |
	| ccpCrosss 	| Point::cross |
	| ccpPerp 		| Point::getPerp |
	| ccpRPerp 		| Point::getRPerp |
	| ccpProject 	| Point::project |
	| ccpRotate 	| Point::rotate |
	| ccpUnrotate 	| Point::unrotate |
	| ccpLengthSQ 	| Point::getLengthSq() |
	| ccpDistanceSQ | Point::getDistanceSq |
	| ccpLength 	| Point::getLength |
	| ccpDistance 	| Point::getDistance |
	| ccpNormalize 	| Point::normalize |
	| ccpForAngle 	| Point::forAngle |
	| ccpToAngle 	| Point::getAngle |
	| ccpClamp 		| Point::getClampPoint |
	| ccpFromSize 	| Point::Point |
	| ccpCompOp		| Point::compOp |
	| ccpLerp 		| Point::lerp |
	| ccpFuzzyEqual | Point::fuzzyEqual |
	| ccpCompMult 	| Point::Point |
	| ccpAngleSigned | Point::getAngle |
	| ccpAngle 		| Point::getAngle |
	| ccpRotateByAngle | Point::rotateByAngle |
	| ccpLineInersect | Point::isLineIntersect |
	| ccpSegmentIntersect | Point::isSegmentIntersect |
	| ccpIntersectPoint | Point::getIntersectPoint |
	| CCPointMake	| Point::Point |
	| CCSizeMake	| Size::Size |
	| CCRectMake 	| Rect::Rect |
	| PointZero 	| Point::ZERO |
	| SizeZero 		| Size::ZERO |
	| RectZero 		| Rect::ZERO |
	| TiledGrid3DAction::tile | TiledGrid3DAction::getTile |
	| TiledGrid3DAction::originalTile | TiledGrid3DAction::getOriginalTile |
	| TiledGrid3D::tile | TiledGrid3D::getTile |
	| TiledGrid3D::originalTile | TiledGrid3D::getOriginalTile |
	| Grid3DAction::vertex | Grid3DAction::getVertex |
	| Grid3DAction::originalVertex | Grid3DAction::getOriginalVertex |
	| Grid3D::vertex | Grid3D::getVertex |
	| Grid3D::originalVertex | Grid3D::getOriginalVertex |
	| Configuration::sharedConfiguration | Configuration::getInstance |
	| Configuration::purgeConfiguration | Configuration::destroyInstance() |
	| Director::sharedDirector() | Director::getInstance() |
	| FileUtils::sharedFileUtils | FileUtils::getInstance |
	| FileUtils::purgeFileUtils | FileUtils::destroyInstance |
	| GLView::sharedOpenGLView | GLView::getInstance |
	| ShaderCache::sharedShaderCache | ShaderCache::getInstance |
	| ShaderCache::purgeSharedShaderCache | ShaderCache::destroyInstance |
	| AnimationCache::sharedAnimationCache | AnimationCache::getInstance |
	| AnimationCache::purgeSharedAnimationCache | AnimationCache::destroyInstance |
	| SpriteFrameCache::sharedSpriteFrameCache | SpriteFrameCache::getInstance |
	| SpriteFrameCache:: purgeSharedSpriteFrameCache | SpriteFrameCache::destroyInstance |
	| NotificationCenter::sharedNotificationCenter | NotificationCenter::getInstance |
	| NotificationCenter:: purgeNotificationCenter | NotificationCenter::destroyInstance |
	| Profiler::sharedProfiler | Profiler::getInstance |
	| UserDefault::sharedUserDefault | UserDefault::getInstance |
	| UserDefault::purgeSharedUserDefault | UserDefault::destroyInstance |
	| Application::sharedApplication | Application::getInstance |
	| ccc3() 		| Color3B() |
	| ccc3BEqual() 	| Color3B::equals() |
	| ccc4() 		| Color4B() |
	| ccc4FFromccc3B() | Color4F() |
	| ccc4f() 		| Color4F() |
	| ccc4FFromccc4B() | Color4F() |
	| ccc4BFromccc4F() | Color4B() |
	| ccc4FEqual() 	| Color4F::equals() |
	| ccWHITE 		| Color3B::WHITE |
	| ccYELLOW 		| Color3B::YELLOW |
	| ccBLUE 		| Color3B::BLUE |
	| ccGREEN 		| Color3B::GREEN |
	| ccRED 		| Color3B::RED |
	| ccMAGENTA 	| Color3B::MAGENTA |
	| ccBLACK 		| Color3B::BLACK |
	| ccORANGE 		| Color3B::ORANGE |
	| ccGRAY 		| Color3B::GRAY |
	| kBlendFuncDisable | BlendFunc::BLEND_FUNC_DISABLE |

# Changes in the Lua bindings

## Use bindings-generator tool for lua binding

Only have to write an ini file for a module, don't have to write a lot of .pkg files

### Bind the classes with namespace to lua

In previous, the lua binding can not bind classes that have the same class name but different namespaces. In order to resolve this issue, now the metatable name of a class is changed. For example, `CCNode` will be changed to `cc.Node`. This modification will affect some APIs as follows:

	|           v2.x                   |                  v3.0             |
	| tolua_usertype(tolua_S,"CCNode") | tolua_usertype(tolua_S,"cc.Node") |
	| tolua_isusertable(tolua_S,1,"CCNode",0,&tolua_err 		| tolua_isusertable(tolua_S,1,"cc.Node",0,&tolua_err  |
	| tolua_isusertype(tolua_S,1,"CCNode",0,&tolua_err) 		| tolua_isusertype(tolua_S,1,"cc.Node",0,&tolua_err)  |
	| toluafix_pushusertype_ccobject(tolua_S, nID, pLuaID, (void*)tolua_ret,"CCNode") 		| toluafix_pushusertype_ccobject(tolua_S, nID, pLuaID, (void*)tolua_ret,"cc.Node")  |
	| tolua_pushusertype(tolua_S,(void*)tolua_ret,"CCFileUtils") 		| tolua_pushusertype(tolua_S,(void*)tolua_ret,"cc.FileUtils")  |
	| tolua.cast(pChildren[i + 1], "CCNode") 			| tolua.cast(pChildren[i + 1], "cc.Node") |

### Use ScriptHandlerMgr to manage the register and unregister of Lua function

When we want to add register and unregister functions of Lua function for class, we need to change the declarative and defined files and then bind to Lua.
In v3.0, we use the `ScriptHandlerMgr`. As an example, lets see the `MenuItem` class:
In the 2.x version, we needed to add a declaration in the MenuItem header file:
```c++
 virtual void registerScriptTapHandler(int nHandler);
 virtual void unregisterScriptTapHandler(void);
```
then implement them in the .cpp file. In the Lua script ,we use it as follow:
```lua
menuItem:registerScriptTapHandler(luafunction)
```

In v3.0 version, we only need to add the `HandlerType` enum in the `ScriptHandlerMgr`, and the implementation in luascript as follow:
```lua
ScriptHandlerMgr:getInstance():registerScriptHandler(menuItem, luafunction,cc.HANDLERTYPE_MENU_CLICKED)
```

## Misc API changes

### Use `cc`、`ccs`、`ccui` `gl` and `sp` as module name

Now classes are binded into different modules instead of using global module. This will avoid conflicts with other codes.

* classes in `cocos2d`、`cocos2d::extension`、`CocosDenshion` and `cocosbuilder`  were bound to `cc` module
* classes in `cocos2d::gui` were bound to `ccui` module
* classes in `spine` were bound to `sp` module
* classes in `cocostudio` were bound to `ccs` module
* global variables are binded to corresponding modules
* all funcionts and constants about `openGl` were bound to `gl` module

Examples:

    | v2.1                    | v3.0                    |
    | CCDirector              | cc.Director             |
    | CCArmature              | ccs.Armature            |
    | kCCTextAlignmentLeft    | cc.kCCTextAlignmentLeft |

### Modified functions

Some global function names are renamed:

Examples:

    | v2.1                    | v3.0                    |
    | CCPoint/ccp             | cc.p                    |
    | CCRect                  | cc.rect                 |
    | CCColor3B               | cc.c3b                  |
    | CCColor4B               | cc.c4b                  |
    | CCColor4F               | cc.c4f                  |

### Add some modules

In the version 3.0, more modules were bound to lua, specific as follows:

* physics
* spine
* XMLHttpRequest
* OpenGL
 
The `XMLHttpRequest` and `physics` are in the `cc` module, the `spine` is in the `sp` module, and the `OpenGl` is in the `gl` module. Related test cases located in:

* physics   ---> TestLua/PhysicsTest
* spine     ---> TestLua/SpineTest
* XMLHttpRequest ---> TestLua/XMLHttpRequestTest
* openGL    ---> TestLua/OpenGLTest

### Add more lua bindings
Such as: New Label、New EventDispatcher and AssetsManager,etc.Related test cases located in:

* New Lable ---> TestLua/LabelTestNew
* New EventDispatcher --->TestLua/NewEventDispatcherTest
* AssetsManager  ---> TestLua/AssetsManagerTest

### Replace some lua-bindings of Class or Struct with lua table
In the version 3.0, all the lua-binding of Struct type were replaced with the lua table

Examples:
 
    | v2.1                    | v3.0                    |
    | CCPoint                 | lua table               |
    | CCRect                  | lua table               |
    | CCColor3B               | lua table               |
    | CCColor4B               | lua table               |
    | CCColor4F               | lua table               |
    | CCAffineTransform       | lua table               |
    | CCArray                 | lua table               |    
    | CCDictionary            | lua table               |
    | CCPointArray            | lua table               |
    
### Support lua script codes call Object-C codes and Java codes 
`LuaObjcBridge` and `LuaJavaBridge` bound to lua surpported lua script codes calls Object-C codes and java codes. 
    
### Add some lua files to store the constants of different modules

* Cocos2DConstants.lua store the constants of `cc` moudle
* StudioConstants.lua store the constants of  `ccs` moudle
* GuiConstants.lua store the constants of `ccui` moudle
* OpenglConstants.lua store the constants of `gl` moudle

