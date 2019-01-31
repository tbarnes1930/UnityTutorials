# Basics of Unity Scripting

When you create a new script from Unity, you get a basic shell of the script. It ends up looking like of the create script:

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour
{
	// Start is called before the first frame update
	void Start () 
	{
		
	}
	
	// Update is called once per frame
	void Update () 
	{
		
	}
}
```

Here's what's happening:

* **"Using" Section** *(lines 1-3)* - This is the libraries that the script will be using
* **"Class" Section** *(lines 5-19)*- Every script contains a script. Your script name is the same as your class. Each class should be unique. Class names should also start with uppercase letter.
* **Class content** - In the basic script that Unity generates for you there are two methods:
	* **Start()** *(lines 9-12)* -  Called when the script starts to run
	* **Update()** *(lines 15-18)*- Called every frame

## Other Methods

Here are a couple other methods that are common in Unity C# scripts

* ``` void Awake () ``` - Called when the script is first called, even if you turn the script off for that Game Object.
* ``` void FixedUpdate () ``` - This is called every time the physics is updated. All physics calculations are done in "one go", to help keep the simulation realistic.
* ``` void LateUpdate () ``` - This is called after `Update()` but right before the camera renders.


## Variables

If you want to have the Unity Editor allow for modifying your class variables from your script, you need to declare them as "public"

```cs
public class BasicScript : MonoBehaviour {
    public int showsUpInUnityEditor = 91;
    int doesNotShowUpUnUnityEditor = 123;

    // Use this for initialization
    void Start () {
    
    }
    
    // Update is called once per frame
    void Update () {
    
    }
}
```

## Debug methods

* ```Debug.Log ( <message> ); ``` -  Similar to print, this is just the Unity way of writing to the console.
* ```Debug.LogWarning ( <message> ); ``` -  Used to display warning messages. You may want to use this to display a notice about maybe not handling a particular event that you didn't complete or something that should not happen under normal conditions.
* ```Debug.LogError ( <message> ); ``` -  With this, if you select *Error Pause* in the Console window when this statement occurs, will pause your game.


## Useful Classes

These class will come up often, which are used in this guide:

```Vector3``` - A class for storing 3D positions and directions. Also has helpers in it to do some standard math.
```Quaternion``` - Represents rotations, more on that later.


## Input

Best practices in Unity state that you should specify your controls through the Input Manager. Here, you can set all the axes you need input on and the user will be able to customize their controllers if need be. To change you inputs, you can select from the menu **Edit -> Project Settings -> Input**. Under each axis, you will have some options that come into play based on what you set:

**Gravity** - The speed in which the axis falls back to 0
**Dead** - The size of the dead zone
**Sensitivity** - The speed in which the value moves toward the target value

```Input.GetAxis ("Axis Name");``` - Returns a float between -1 and 1. This will give you the value of the "Axis Name" as defined in **Edit -> Project Settings -> Input**. 

```Input.GetAxisRaw ("Axis Name");``` - Returns a float that is either -1, 0, or 1. This only returns absolute values, there is no in-between.

As with an axis, you can also check buttons, the same way:

```Input.GetButton ("Button Name");``` - Returns a boolean. If the button is being held down this will be true

```Input.GetButtonDown ("Button Name");``` - Returns a boolean. When the button is first pressed down, this will be true

```Input.GetButtonUp ("Button Name");``` - Returns a boolean. When the button is released, this will be true

Unity has an Input class which has a lot of methods for getting controllers/keys/mouse input besides using these specified axes.

```Input.GetKey ( [ KeyCode or "Button Name" ]);``` - Returns a boolean. If the key is being held down this will be true

```Input.GetKeyDown ( [ KeyCode or "Button Name" ] );``` - Returns a boolean. When the key is first pressed down, this will be true

```Input.GetKeyUp ( [ KeyCode or "Button Name" ] );``` - Returns a boolean. When the key is released, this will be true

For mouse use, for buttonNumber the values are: 0 - left button, 1 - right button, 2 - middle button

```Input.GetMouseButton (buttonNumber);``` - Returns a boolean. If the mouse button is being held down this will be true

```Input.GetMouseButtonDown (buttonNumber);``` - Returns a boolean. When the mouse button is first pressed down, this will be true

```Input.GetMouseButtonUp (buttonNumber);``` - Returns a boolean. When the mouse button is released, this will be true

For accelerometers, like in phones and tablet, you can use:

```Input.GetAccelerationEvent(index);``` - Returns an AccelerationEvent, which contains a Vector3 with the value of acceleration and the deltaTime since the last measurement, 

While this shows up in the Input class, the following is only useful for showing the names of Joysticks:

```Input.GetJoystickNames();``` - Returns a list of Joysticks

## Changing objects

To change objects in script, you can do the following 

```transform.localScale = new Vector3 (1.0f, 0.5f, 1.0f);``` - adjust the scale of the object by 0.5 units on the Y axis, both the Z and Z will stay "normal". 

```transform.position = new Vector3 (10.0f, 10.0f, 10.0f);``` - Changes position of the object to 10,10,10

To change the rotation, the easiest way to do so is with the Rotate().

```transform.Rotate(new Vector3 (0.0f, 0.0f, 5.0f));``` - Rotates an object on the Z axis

```transform.rotation = new Quaternion ();``` - Rotates back to 0,0,0

To rotate to a particular position, you need to use a little [Quaternions](http://docs.unity3d.com/ScriptReference/Quaternion.html) magic:
```cs
Quaternion fromRotation = transform.rotation;
Quaternion toRotation = Quaternion.Euler (10.0f, 10.0f, 10.0f);
transform.rotation = Quaternion.Lerp (fromRotation, toRotation, 1.0f);
```

## Randomness

Want a random number? Try this:

```Random.value``` - Returns a float - random number between 0.0 and not exactly 1

```Random.Range(0.0f, 20.0f)``` - Returns a random number between the values you set. If you don't use a decimal, the numbers will be integers.

## Lights

Want to modify a light? It's easy to do:

```gameObject.GetComponent<Light>().intensity = Random.Range (0.0f, 8.0f);``` - Changes the intensity of a light. WARNING: may cause seizures if you run this in an Update()!

## Targeting Game Objects

By default, if you try these commands, you can only do it on the current Game Object. You could also make a public Game Object variable in a class, like a game controller class, and perform these command on that:

```cs
public GameObject objectToManipulate;
```
...

```cs
objectToManipulate.transform.Rotate(new Vector3 (10.0f, 10.0f, 10.0f));
```

## Prefabs

* **Prefab** - A reusable Game Objects that you create
* **Instance** - One of the actual objects in a scene
* **Instantiate** - Process of creating an Instance
* **Inheritance** - How the Prefab is linked to all the instances

Starting with Unity 2018.3, Prefabs have gone through an important change. You can now edit Prefabs in it's own mode, sort of removing them from your game world and allowing you to edit them as it's own object in the editor!

![](https://blogs.unity3d.com/wp-content/uploads/2018/06/image1-5.png)

To make Prefabs, create a folder in your **Assets** named **Prefab**. Once you do that, drag an item from the Hierarchy window into the **Assets -> Prefab** folder and you have a *Prefab*.

To create an instance of the Prefab, just drag the Prefab from the Project tab into the scene.

If you want to stop an instance of a Prefab from being linked to the Prefab, from the menu you can select **GameObject -> Break Prefab Instance**.

Later in the semester we will talk more about prefab variants and overrides.

## Instantiate

Now you can create your object through code using the `Instantiate();` method. To do so:

1. Create a Prefab

2. Create an empty game object and call it ***SpawnPoint***

3. Create this spawner script, which will create a new prefab in the same spot that it was built in when you press backspace, and at the location of the ***SpawnPoint*** when you press the spacebar:

	```
	using UnityEngine;
	using System.Collections;
	
	public class Spawner : MonoBehaviour {
	    public GameObject prefab;
	
	    void Update () {
	        if (Input.GetKeyDown (KeyCode.Backspace)) {
	            Instantiate (prefab);
	        } else if (Input.GetKeyDown (KeyCode.Space)) {
	            Instantiate (prefab, this.transform.position, this.transform.rotation);
	        }
	    }
	}
	```

4. Attach the script to ***SpawnPoint***

5. Drag the prefab you want to the “Prefab” field under ***SpawnPoint***’s script

## Collision

Collision requires a collider. By default, the basic shapes in Unity have them. You can modify the size of them if you need, or add custom collision maps.

Colliders provide a few Triggers to help you do things. Once you turn them on with the "Is Trigger" checkbox, you can do things like:

```void OnTriggerEnter ( Collider other )``` - Called when the object first enters the trigger

```void OnTriggerStay ( Collider other )``` - Called every update the object is in the trigger

```void OnTriggerExit ( Collider other )``` - Called when an object leaves a trigger

All of these have a parameter passed that is the object that enter/is in/left the trigger.

## Rigidbody

To change a rigidbody's speed, you can do:

```rigidbody.velocity = new Vector3 (13.4f, 0.0f);``` - This will make it move 13.4 units on the X and 0 on the Y.

You can also change other aspects of the rigidbody, remember the options in the Inspector View? They are just variables: 

```cs
rigidbody.mass = 1;
rigidbody.drag = 0;
rigidbody.angularDrag = 0.05;
rigidbody.useGravity = true;
rigidbody.isKinematic = false;
rigidbody.interpolation = RigidbodyInterpolation.None;
rigidbody.collisionDetectionMode = CollisionDetectionMode.Discrete;
rigidbody.constraints = RigidbodyConstraints.None
```

**NOTE 1**: interpolation and collisionDetectionMode use an Enums to have you choose what you want. Visual Studio (and even MonoDevelop) will help you make the selection of the options.

**NOTE 2**: constraints is integer, using bit fields where you have to use a bitwise or "|" to join together which axis you want to freeze. If you want to freeze the Y axis for position and freeze rotation on the X axis would need to do the following:

```cs
rigidbody.constraints = RigidbodyConstraints.FreezePositionY | RigidbodyConstraints.FreezeRotationX;
```

## Looking Ahead

To figure out if an object is in front of you, you can raycast. There are 12 different ways to call Physics.Raycast. This one looks 10 units in front of an object:

```cs
bool objectThere = Physics.Raycast (transform.position, transform.forward, 10.0f);
```

This will allow you to tell which object you hit (and destroy it!!)

```cs
RaycastHit objectHit;
if (Physics.Raycast (transform.position, transform.forward, out objectHit)) {
    Debug.Log ("Destory " + objectHit.collider.gameObject.name + "!!!!");
    Destroy (objectHit.collider.gameObject);
}
```


