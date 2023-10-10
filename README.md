# Create-A-FPS-Controller
 
In this Tutorial, I will show you how to make an FPS controller in under a minute based on the Tutorial I've watched on YouTube.

## 1. Create a Unity project

Start creating a Script folder and then write a script called `FPSController` and add it to the folder.

## 2. Open up the script with Visual Studio and add these codes in

Before you add movenet you add this on top of the public class `FPSController/Whatever you named it on your project`.
```.cs
[RequireComponent(typeof(CharacterController))]
```

Then you add the game object node for the camera below the public class name, the movement nodes.
```.cs
{
    public Camera playerCamera;
    public float walkSpeed = 6f;
    public float runSpeed = 12f;
    public float jumpPower = 7f;
    public float gravity = 10f;
```

Then the Mouse looks at limit nodes including a bool variable and names it `canMove` and makes sure you set it to true, To prevent them from looking too high or too low.

```.cs
    public float lookSpeed = 2f;
    public float lookXLimit = 45f;

    Vector3 moveDirection = Vector3.zero;
    float rotationX = 0;

    public bool canMove = true;
```

Then you add this variable to make the script work properly.

```.cs
    CharacterController characterController;
```

Once you have done that you can then add the hide cursor and lockmode in the Void Start script.
```.cs
    void Start()
    {
        characterController = GetComponent<CharacterController>();
        Cursor.lockState = CursorLockMode.Locked;
        Cursor.visible = false;
    }
```

Then in Void update add the region in it.

First the Movement.

```.cs
#region Handles Movement
        Vector3 forward = transform.TransformDirection(Vector3.forward);
        Vector3 right = transform.TransformDirection(Vector3.right);
```
Then the Sprint System.
```.cs
        //Press Left Shift to run
        bool isRunning = Input.GetKey(KeyCode.LeftShift);
        float curSpeedX = canMove ? (isRunning ? runSpeed : walkSpeed) * Input.GetAxis("Vertical") : 0;
        float curSpeedY = canMove ? (isRunning ? runSpeed : walkSpeed) * Input.GetAxis("Horizontal") : 0;
        float movementDirectionY = moveDirection.y;
        moveDirection = (forward * curSpeedX) + (right * curSpeedY);

        #endregion
```

Then the Jumping and make sure you follow it correctly.
```.cs
#region Handles Jumping
        if (Input.GetButton("Jump") && canMove && characterController.isGrounded)
        {
            moveDirection.y = jumpPower;
        }
        else
        {
            moveDirection.y = movementDirectionY;
        }

        if (!characterController.isGrounded)
        {
            moveDirection.y -= gravity * Time.deltaTime;
        }

        #endregion
```

And last but not least the rotation. (Which I forgot to add at first when I first started the game)
```.cs
#region Handles Rotation
        characterController.Move(moveDirection * Time.deltaTime);

        if (canMove)
        {
            rotationX += -Input.GetAxis("Mouse Y") * lookSpeed;
            rotationX = Mathf.Clamp(rotationX, -lookXLimit, lookXLimit);
            playerCamera.transform.localRotation = Quaternion.Euler(rotationX, 0, 0);
            transform.rotation *= Quaternion.Euler(0, Input.GetAxis("Mouse X") * lookSpeed, 0);
        }

        #endregion
    }
```

## 3. Then add a player to your game.

First, create an Empty GameObject and call it `Player`, add a capsule component, and then add the `Main Camera` into the player game object.

Once you do that add the `FPSController` Script into `Player` and the play test it. and there you have it.

A player that moves, Jump and Sprint.

Hope this was useful and let me know what you think.

Special thanks to Noblob for helping with the movement.

The Channel link will be Below.

Copy and paste or Click on the link.
https://www.youtube.com/@NoblobRS
