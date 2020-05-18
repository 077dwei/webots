# Development

This is an archive of the `development` channel of the [Webots Discord server](https://discordapp.com/invite/nTWbN9m).

## 2020

##### David Mansolino [cyberbotics] 05/15/2020 04:57:11
`@Sanket Khadse` the controller of the drone is very simple, it doesn't do any feedback using the GPS position or any inertial unit, it might therefore easily drift.


`@Jesusmd` you want to get the type of distance sensor? If so you should use the ``getType`` function: [https://cyberbotics.com/doc/reference/distancesensor?tab-language=python#wb\_distance\_sensor\_get\_type](https://cyberbotics.com/doc/reference/distancesensor?tab-language=python#wb_distance_sensor_get_type)

##### Sanket Khadse 05/15/2020 03:02:49
Hey, may I know, why does a drone, (let it be DJI Mavic 2 Pro, which is already available into Webots, or a custom made) moves up and down and shifts constantly in one direction even if no such behaviour is defined in the controller code?

##### Jesusmd 05/15/2020 01:49:44
`@David Mansolino` Hi, I am using python, I would like to work with several distance sensors at the same time and comparing their data in console. But instead of obtain the type, I got a number.

##### Luftwaffel 05/14/2020 12:48:46
> `@Luftwaffel` Although working with ROS, we decided to split the simulation projects with minimal dependencies over ROS other than communication. For 3D Math in Cpp, we're using Eigen, and in python either numpy or numpy + transformations.py (which is standalone of tf)

`@Axel M` 

Awesome, thank you so much. That makes things easier

##### Axel M [Premier Service] 05/14/2020 09:11:01
The integration with ROS is super easy too with [http://wiki.ros.org/eigen\_conversions](http://wiki.ros.org/eigen_conversions) 🙂


Regarding your example of getting relative position between two nodes, that can be easily achieved with Eigen in cpp (3x3 Matrix -> Quaternion, quaternion / vector product)


`@Luftwaffel` Although working with ROS, we decided to split the simulation projects with minimal dependencies over ROS other than communication. For 3D Math in Cpp, we're using Eigen, and in python either numpy or numpy + transformations.py (which is standalone of tf)

##### David Mansolino [cyberbotics] 05/14/2020 05:34:23
`@Luftwaffel` instead of relying on ROS, if you are using Python you ca probably use the `transforms3d` python package which allows for example to convert from a rotation matrix to quaternions: [https://matthew-brett.github.io/transforms3d/reference/transforms3d.quaternions.html#transforms3d.quaternions.mat2quat](https://matthew-brett.github.io/transforms3d/reference/transforms3d.quaternions.html#transforms3d.quaternions.mat2quat)

##### Luftwaffel 05/13/2020 18:04:16
`@Sanket Khadse`  perhaps direct .proto file edit can help

##### Sanket Khadse 05/13/2020 17:11:36
`@Luftwaffel` that is what my problem is about. The nodes I have to copy paste one by one are in "hundreds".

##### Luftwaffel 05/13/2020 16:47:12
Perhaps add a 'Group' base node, put all your nodes in, and copy paste that

##### Sanket Khadse 05/13/2020 16:45:26
Oh, thank you for letting me know!
Keep it as a suggestion for the next update. 😄

##### Olivier Michel [cyberbotics] 05/13/2020 16:44:11
Unfortunately, this is not possible.

##### Sanket Khadse 05/13/2020 16:43:42
Hey, could you tell me how to multi-select things in scene-tree? 
I can't find a way to. I imported a VRML97 model into Webots, the model was quite large, and it's difficult to select, cut and paste each 'transform' object into a robot node's children attribute.

##### Luftwaffel 05/13/2020 16:21:05
I'll give it a try


oh lordy 😅

##### Olivier Michel [cyberbotics] 05/13/2020 16:20:18
Yes, if you have an idea to achieve this... Now you know how to open a pull request 😉

##### Luftwaffel 05/13/2020 16:19:08
yes, but it requires runtime.ini edits. Would be nice if people can have it run as an example out of the box

##### Olivier Michel [cyberbotics] 05/13/2020 16:18:13
If you use Webots with ROS, you need to install ROS, and you should get 'tf' for free, isn't it?

##### Luftwaffel 05/13/2020 16:16:44
it is almost a must if working with ROS and robots


how big of a deal would it be to add the 'tf' package to the webots ROS environment?

##### Olivier Michel [cyberbotics] 05/13/2020 16:15:26
Oops...

##### Luftwaffel 05/13/2020 16:15:08
Problem is, the link to their liscense is broken 😅


[https://sscc.nimh.nih.gov/pub/dist/bin/linux\_gcc32/meica.libs/nibabel/quaternions.py](https://sscc.nimh.nih.gov/pub/dist/bin/linux_gcc32/meica.libs/nibabel/quaternions.py)


specifically this:


[https://nipy.org/nibabel/reference/nibabel.quaternions.html](https://nipy.org/nibabel/reference/nibabel.quaternions.html)


The thing is, I want it to be able to run in the native webots environment, so no extra packages. I got it to work using their code:

##### Olivier Michel [cyberbotics] 05/13/2020 16:13:18
OK, so you need to convert this 3x3 rotation matrix to a quaternion then.


Yes, you are right. Sorry, my bad.

##### Luftwaffel 05/13/2020 16:12:12
but that returns the 3x3 matrix

##### Olivier Michel [cyberbotics] 05/13/2020 16:11:40
If you need absolute rotation (in axis-angle notation), use:  [https://www.cyberbotics.com/doc/reference/supervisor#wb\_supervisor\_node\_get\_orientation](https://www.cyberbotics.com/doc/reference/supervisor#wb_supervisor_node_get_orientation)


(gives relative rotation in axis-angle notation)


[https://www.cyberbotics.com/doc/reference/supervisor#wb\_supervisor\_field\_get\_sf\_rotation](https://www.cyberbotics.com/doc/reference/supervisor#wb_supervisor_field_get_sf_rotation)

##### Luftwaffel 05/13/2020 16:09:36
how do we get axis angles? I have the 3x3 matrix

##### Olivier Michel [cyberbotics] 05/13/2020 16:09:02
```python
def axis_angle_to_quaternion(axis, theta):
    axis = numpy.array(axis) / numpy.linalg.norm(axis)
    return numpy.append([numpy.cos(theta/2)], numpy.sin(theta/2) * axis)
```


No, you have to get it as an axis-angle representation, but that's super easy to translate into quaternion.

##### Luftwaffel 05/13/2020 16:04:22
Btw, is there a way to get the quaternion orientation directly from webots? That would make things much simpler


Still a bit crude with little error correction, but it works 🙂


<@&568329906048598039> Btw, I'm currently working on a python program, that publishes the PoseStamped of one or more nodes into a rostopic. It also allows to publish the Pose relative to a specific node.

##### David Mansolino [cyberbotics] 05/11/2020 06:43:54
> Is there an quickly way to get the attribute instead of  predefined values as for example in  Keyboard class.?  I would preffer enum as in c or c++

`@Jesusmd` which language are you using?

##### Jesusmd 05/10/2020 06:29:26
Is there an quickly way to get the attribute instead of  predefined values as for example in  Keyboard class.?  I would preffer enum as in c or c++

##### Luftwaffel 04/29/2020 22:31:07
[https://www.andre-gaschler.com/rotationconverter/](https://www.andre-gaschler.com/rotationconverter/)

##### Shubham D 04/29/2020 22:30:02
I am trying to create some solid using transform and shapes.
I am not able to set the objects at desired angle.
How should I set the parameters if I need specific angle.
Or is their any simple way to convert normal vector direction to euler angles

##### David Mansolino [cyberbotics] 04/28/2020 14:09:41
You're welcome

##### Psyka 04/28/2020 14:06:55
it working fine


very nice thank's 🙂

##### David Mansolino [cyberbotics] 04/28/2020 14:04:40
You should have:
> Create\_Webots\_World\wbt\worlds\textures\ground.jpg


##### Psyka 04/28/2020 14:04:40
so I just have to rename it ?


I understand now what you mean

##### David Mansolino [cyberbotics] 04/28/2020 14:04:24
yes indeed

##### Psyka 04/28/2020 14:04:20
ho ok


?


worlds


how should I name that directory ?


it was working fine like last week

##### David Mansolino [cyberbotics] 04/28/2020 14:03:18
It's still problematic 😉

##### Psyka 04/28/2020 14:03:05
it's "Created\_World" and not "Created World" I just writte it wrong

##### David Mansolino [cyberbotics] 04/28/2020 14:01:36
because the names in the project folder should respect a strict convention (you can name the project folder as you want but not the folders inside):
   [https://cyberbotics.com/doc/guide/the-standard-file-hierarchy-of-a-project](https://cyberbotics.com/doc/guide/the-standard-file-hierarchy-of-a-project)

##### Psyka 04/28/2020 14:01:01
is a copy past of my path


Create\_Webots\_World\wbt\Created\_World\textures\ground.jpg


yes

##### David Mansolino [cyberbotics] 04/28/2020 14:00:36
But that's not what you said:
> Create\_Webots\_World\wbt\Created\_World\textures\ground.jpg


##### Psyka 04/28/2020 14:00:35
hooo why that can't it be ?


and all my .wbt are in worlds


exactly

##### David Mansolino [cyberbotics] 04/28/2020 13:59:47
Ok, this is the problem, the folder in which are the world can't be named 'Created World'.
You should have:
```
Create_Webots_World\wbt\
                     -> controllers
                     -> worlds
                           -> textures
                                    -> ground.jpg
```

##### Psyka 04/28/2020 13:57:47
Create\_Webots\_World\wbt\Created\_World\textures\ground.jpg


there I've got a directory "controllers" for controllers and "Created World" with in it "textures" and then the "ground.jpg"


so all my project are in E:\...\...\Project\wbt\


haaa sorry I'm loosing it


wbt directory


and inside I've got a directory named "textures"


I've got a directory named "Created World" where there is all my .wbt worlds using this

##### David Mansolino [cyberbotics] 04/28/2020 13:53:08
And where is it in your project directory exactly?

##### Psyka 04/28/2020 13:52:57
copy pasted


ground


textures

##### David Mansolino [cyberbotics] 04/28/2020 13:52:11
Is it correctly in a ``textures``  folder?

##### Psyka 04/28/2020 13:51:50
and it's not working


but the file is int the current directory of the current project


WARNING: DEF GROUND Solid > Shape > PBRAppearance > ImageTexture: 'textures/ground.jpg' not found.
A resource file can be defined relatively to the worlds directory of the current project, relatively to the worlds directory of the default project, relatively to its protos directory (if defined in a PROTO), or absolutely.


I've got an other issue :


Hey again,

##### mint 04/26/2020 14:54:05
Thank you Olivier!

##### Olivier Michel [cyberbotics] 04/26/2020 14:44:13
See [https://cyberbotics.com/doc/reference/supervisor#wb\_supervisor\_node\_get\_orientation](https://cyberbotics.com/doc/reference/supervisor#wb_supervisor_node_get_orientation)


It's a rotation matrix (as in OpenGL).

##### mint 04/26/2020 14:38:09
One question while developing.. what is the convention webot's using for the matrix returned by wb\_supervisor\_node\_get\_orientation function?
is it Euler Angles, Rotation Matrix, or Tait–Bryan angles?

##### David Mansolino [cyberbotics] 04/23/2020 07:38:41
You're welcome

##### fdvalois 04/23/2020 07:38:11
Thank you!

##### David Mansolino [cyberbotics] 04/23/2020 07:37:52
A breaitenberge already compatible with many robot is distributed with Webots, here is the code:  [https://github.com/cyberbotics/webots/tree/master/projects/default/controllers/braitenberg](https://github.com/cyberbotics/webots/tree/master/projects/default/controllers/braitenberg)

##### fdvalois 04/23/2020 07:34:46
You just look braitenberg controller online?

##### David Mansolino [cyberbotics] 04/23/2020 05:44:20
Hi `@imjusta23` you might simply use the braitenberg controller.

##### imjusta23 04/22/2020 20:20:17
Any advice about how to get the epuck moves randomly in this world
%figure
![image0.jpg](https://cdn.discordapp.com/attachments/565155651395780609/702614779767947394/image0.jpg)
%end


##### Dorteel 04/21/2020 09:13:59
Thank you `@Stefania Pedrazzi` ! 🙂

##### Stefania Pedrazzi [cyberbotics] 04/20/2020 06:14:54
`@Dorteel`, no there is no automatic procedure to import Choregraphe built-in motions in Webots. But if you have the motion joints values, then you should be able to reproduce it in Webots.

##### Dorteel 04/18/2020 05:48:34
Hi, I have a question regarding the Nao robot. I saw WeBots used to be able to interface with Choreographe, I was wondering if the motions for the Nao that are built-in in Choreographe can somehow be imported into WeBots?

##### David Mansolino [cyberbotics] 04/16/2020 07:15:29
You're welcome

##### Aan 04/16/2020 07:14:58
thank you 😀

##### David Mansolino [cyberbotics] 04/16/2020 07:13:19
Here is the  fix:
[https://github.com/cyberbotics/webots/pull/1548/files](https://github.com/cyberbotics/webots/pull/1548/files)
It will be available in the next release of Webots. But in the meantime, you might apply it locally to your Webots installation files.

