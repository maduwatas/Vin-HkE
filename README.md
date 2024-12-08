# Vinapp HkE

Vinapp HkE is an extension implementation for Hammerhead Karroo based on Vin's CORE and the new Hammerhead SDK for its GPS devices. Tested in Hammerhead K2 and K3 devices.

# HkE app Navigation

### Main menu

![Alt text](./images/menu.png?raw=true "Menu")

### User profile configuration





User profile | Power threshold data | Hear rate threshold data | Nutritional data
------------- | ------------- | ------------- | -------------
![](./images/usuario2.png?raw=true "User data") | ![](./images/power.png?raw=true "User data") | ![](./images/hr.png?raw=true "Threshold data") | ![](./images/nutricion.png?raw=true "Threshold data")

Some data is imported from the karoo user profile. This data is not editable, other data can be. The user's gender, weight, height and threshold data are necessary to adjust the calculations of calories burned in the activity. The nutritional data provides information on these calculations, for different intensities.

### Alerts

Alert list | Create or update alert 
------------- | -------------
![](./images/alertas.png?raw=true "Alerts") | ![](./images/alertas2.png?raw=true "Alertsmas")

Vin HkE allows you to configure different alerts with sound warnings and pop-up texts. 

Using the + button the user can add a new alert.

By clicking on a pre-existing alert it is possible to modify it. Within the detail window of an alert, the X button exits without saving the changes.

Existing alerts can be activated or deactivated by pressing the bell button.

To delete an alert, long press and when the delete icon appears, press it.

### Box chalkboard

Chalkboard submenu | Message list | Message detail | Remove message
------------- | ------------- | ------------- | -------------
![](./images/box.png?raw=true "Box") | ![](./images/mensajes.png?raw=true "mesagge") | ![](./images/mensajes2.png?raw=true "mesagge2") | ![](./images/borrarMensaje.png?raw=true "mesagge3")

Mesagge mass

### Vinapp Workout

![Alt text](./images/workout.png?raw=true "workout")

Workout


# Extension widgets

## Calories estimated by Heart Rate

## Graphical Power

## Graphical Hear rate

## Graphical Power balance

## Calorie consumption

## Box Chalkboard 

# SDK Implementation details

It is necessary to have the dependency on Hammerhead extensions for Vinapp HkE to work.

implementation("io.hammerhead:karoo-ext:1.x.y")

At the time of writing this document the extensions library is not public, so the configuration files load a local lib that is not included in this git.

## Basic mechanics
When your karoo starts up, look in the manifests of the apps you have installed for the definition of the extension service.

<service
    android:name="es.xproject.vin.hammerhead.extension.HammerheadExtension"
    android:exported="true"
    tools:ignore="ExportedService">
    <!-- Required for this extension to be discovered by the Karoo System -->
    <intent-filter>
        <action android:name="io.hammerhead.karooext.KAROO_EXTENSION" />
    </intent-filter>
    <!-- Required for this extension to define resources and definitions -->
    <meta-data
        android:name="io.hammerhead.karooext.EXTENSION_INFO"
        android:resource="@xml/extension_info" />
</service>

Here we indicate which class must be executed to start your extension and in which file the data fields it includes are defined.

## HammerheadExtension main class
When the Vin HkE extension starts up it queries your user profile in the karoo, sets listeners for your activity status, starts collecting data from your devices and stores it in a singleton object, the DeviceHandler.

## File extension_info
In this file you indicate the data fields that your extension includes. For each field you include you must indicate a name, description and an icon. The fields in which you are only going to show a numerical value will be graphical false, the rest will be graphical true
Finally, you must indicate the typeId attribute with a **unique identifier** that will help karoo identify which view it should show when the user selects this field. For example

**typeId="custom-calories"** in the xml

class CustomCaloriesDataType(
private val karooSystem: KarooSystemService,
extension: String,
) : **DataTypeImpl(extension, "custom-calories")** in view data type class

## Views
Extension views are implemented by two classes. The Typo class collects the data and updates the view's Composable. The Composable class creates the view itself from data collected directly by its associated type.
Extensions are developed to consume composables that must be built with Glance. In my example I have used Glance only to return a container in which I insert Remoteviews developed in the classic way, that is, with xml layouts. I have taken this option because it is the one I master, but the normal thing would be to use glance. One of the problems with my approach is that you have to control the theme manually, to change the colors of the texts.


## Vinapp FIT Manager
When you start an activity, an object is also started in the Vinapp core that stores all the instant data sent by your devices in records with a FIT structure.
The FIT object can later be used in all the extensions you design without needing to establish dependencies on all the necessary devices. This way your extensions can work even if not all the data they display is being collected. That is, views can use the data collected by their associated Type or consume the DeviceHandler object provided by Vinapp.
The FIT object allows complex calculations from the collected data.

## APK
Download Vinapp HkE here: https://github.com/maduwatas/Vin-HkE/releases/download/latest/Vin-HkE.apk



