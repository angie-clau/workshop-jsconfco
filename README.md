# Nativescript + Angular 2 workshop for JSConf Colombia

> This workshop assumes you have already set up your system for Mobile development. 
> If not, please refer to http://docs.nativescript.org/angular/start/quick-setup

### Instructions

1. Install Nativescript. Open Terminal and type:

	``` sh
	npm i -g nativescript 
	```

2. Open Terminal/CMD, create a Nativescript workshop folder and clone the project:

	``` sh
	git clone https://github.com/angie-clau/workshop-jsconfco.git
	```
3. Let's compile the initial files for IOS and Android. Go to the workshop folder:
	
	``` sh
	cd workshop-jsconfco
	```
4. Run:
	``` sh
	npm install
	````
and then:
	``` sh
	tns build android
	tns build ios
	```

5. Now let's run our base app on the emulator/device:

	``` sh 
	tns run android --emulator / tns run android
	tns run ios --emulator / tns run ios
	```
6. Now, let's go and listen our app changes. If your previous tns run ios or tns run android task is still running, type **Ctrl+C** in your terminal to kill it. And now type:

	``` sh 
	tns livesync android --emulator --watch / tns livesync android --watch 
	tns livesync ios --emulator --watch / tns livesync ios --watch
	```

7. Let's go and create our  first page, copy and paste the next code inside your `app/pages/login/login.ts`

	``` javascript 
	import {Component} from "@angular/core";
	import { Router } from "@angular/router";
	import {Page} from "ui/page";
	@Component({
			selector: "my-app",
			templateUrl: "pages/login/login.html"
	})
	export class LoginComponent {
		constructor(private page: Page) {
			page.actionBarHidden = true;
		}
	}
	```

8. Now create our Login html Template. copy and paste the next code inside your `app/pages/login/login.html` file:

	``` html
	<GridLayout rows="*,*,*,*" columns="*" verticalAlignment="center" horizontalAlignment="stretch">
		<!-- Text -->
		<Label row="0" col="0" text="Login" textWrap="true" class="login-title"></Label>

		<!-- Email Input -->
		<AbsoluteLayout row="1" col="0" class="fontAwesome-icons">
			<Label text="&#xf007;"></Label>
			<TextField hint="Email Address" keyboardType="email" autocorrect="false" autocapitalizationType="none"></TextField>
		</AbsoluteLayout>

		<!-- Password Input -->
		<AbsoluteLayout row="2" col="0" class="fontAwesome-icons margin-bottom-m">
			<Label text="&#xf023;"></Label>
			<TextField hint="Password" secure="true"></TextField>
		</AbsoluteLayout>

		<!-- Login Btn -->
		<Button row="3" col="0" text="Ingresar" class="btn-fontSize btn-positive"></Button>
	</GridLayout>
	```

9. Now, let's add the routing logic. Copy and paste the next code inside your `app/app.routing.ts`:

	``` javascript
	import { LoginComponent } from "./pages/login/login";

	export const routes = [
		{ path: "", component: LoginComponent }
	];

	export const navigatableComponents = [
		LoginComponent
	];
	```

10. Now, open your `app/app.component.ts` and paste the following tag <page-router-outlet>, which is your app first directive:

	``` javascript
	import { Component } from "@angular/core";

	@Component({
		selector: "main",
		template: "<page-router-outlet></page-router-outlet>"
	})
	export class AppComponent {}
	```

11. Now, let's go and add the routes for our app. Copy and paste the next code inside your `app/main.ts`:

	``` javascript
	import { platformNativeScriptDynamic, NativeScriptModule } from "nativescript-angular/platform";
	import { NgModule } from "@angular/core";
	import { NativeScriptFormsModule } from "nativescript-angular/forms";
	import { AppComponent } from "./app.component";
	import { NativeScriptRouterModule } from "nativescript-angular/router";
	import { routes, navigatableComponents } from "./app.routing";

	@NgModule({
		declarations: [
			AppComponent, 
			...navigatableComponents
		],
		bootstrap: [AppComponent],
		imports: [
			NativeScriptModule,
			NativeScriptFormsModule,
			NativeScriptRouterModule, 
			NativeScriptRouterModule.forRoot(routes)
		],
	})
	
	class AppComponentModule {}

	platformNativeScriptDynamic().bootstrapModule(AppComponentModule);
	```

12. Now, our Login page doesn't look that pretty, does it? So let's go and add some styling to it. If your previous tns livesync task is still running, type **Ctrl+C** in your terminal to kill it. Here is where we add our first tns plugin, go to the project root inside your terminal and paste:

	``` sh
	tns install sass
	```

	Now let's watch our app changes again, type:

	``` sh
	tns livesync android --emulator --watch / tns livesync android --watch
	tns livesync ios --emulator --watch / tns livesync ios --watch
	```

13. Now, let's create a partial file to use and import in our app. Open your `app/styles/_variables.scss` and paste the following code:

	``` scss
	//Colors
	$primary-gray: #eeeeee;
	$secondary-gray: #969696;
	$button-blue: #29526D;
	$button-green: #1CA59F;
	$button-orange: #c9510c;
	$red: #ff0000;

	//Fonts
	$fontSize-btn: 16px; 
	```

14. Now let's call those global variables and use them. Open your app Sass file `app/app.scss` and paste:

	```scss
	@import 'styles/_variables.scss';
	.fontAwesome-icons {
		font-size: 20;
		font-family: 'FontAwesome'
	}

	TextField,
	TextView {
		border-width: 1;
		border-color: $primary-gray;
	}

	.btn-fontSize {
		font-size: $fontSize-btn;
	}

	.btn-positive {
		background-color: $button-blue;
		color: white;
	}
	.btn-green {
		background-color: $button-green;
		color: white;
	}

	.margin-m {
		margin: 10;
	}
	.margin-noTop-m{
		margin: 0 10;
	}
	.margin-l {
		margin: 15;
	}
	.margin-left-s {
		margin-left: 5;
	}

	.margin-top-m {
		margin-top: 10;
	}
	.margin-bottom-s {
		margin-bottom: 5;
	}

	.margin-bottom-m {
		margin-bottom: 10;
	}

	.padding-m {
		padding: 10;
	}
	.padding-left {
		padding-left:15;
	} 
	```

15. Hmmmm... it's not looking so good :/ so let's go and add some specific styles for our Login page. Open your `app/pages/login/login.ts` and copy and paste the following:

	``` javascript
	import {Component} from "@angular/core";
	import { Router } from "@angular/router";

	@Component({
		selector: "my-app",
		templateUrl: "pages/login/login.html",
		styleUrls: ['pages/login/login-common.css']
	})

	export class LoginComponent {
		constructor(private page: Page) {
			page.actionBarHidden = true;
		}
	}
	```

	and then add the styles inside your `app/pages/login/login-common.scss` Sass file:

	``` scss
	@import 'styles/_variables.scss';

	GridLayout {
		margin: 50;
	}

	Button {
		border-radius: 5;
		width: 70%;
		height: 50;
	}

	TextField {
		padding-left: 35;
		height: 50;
		width:100%;
	}

	Label {
		top: 15;
		left: 10
	}
	```

16. Hmmmm... but that "login" Label doesn't look that pretty, what if we add some platform specific styles? Open your `app/pages/login/login.ts` and paste:

	``` javascript  
	import {Component} from "@angular/core";
	import { Router } from "@angular/router";

	@Component({
		selector: "my-app",
		templateUrl: "pages/login/login.html",
		styleUrls: ['pages/login/login-common.css', 'pages/login/login.css']
	})

	export class LoginComponent {
		constructor(private page: Page) {
				page.actionBarHidden = true;
		}
	}
	```

	Then, open your `app/pages/login/login.android.scss` and paste the following code:

	```scss
	@import 'styles/_variables.scss';

	.login-title{
		font-size: 26;
		color: $button-orange;
		text-align:center;
		margin-bottom: 20;
	}
	```

	and finally, open your `app/pages/login/login.ios.scss` and paste the following code:

	```scss
	@import 'styles/_variables.scss';

	.login-title{
		font-size: 26;
		color: $button-blue;
		text-align:center;
		margin-bottom: 20;
	}
	```

17. We have our Login page looking really pretty, so, now we'll have to create a new page to navigate after we press the Login btn. Let's go and create a new Character component, open your `app/pages/characters/characters.ts` and paste:

	```javascript
	import {Component, ElementRef, OnInit, ViewChild} from "@angular/core";
	import * as application from "application";
	import * as platform from "platform";

	@Component({
		selector: 'characters-list',
		templateUrl: 'pages/characters/characters.html',
		styleUrls: ['pages/characters/characters.css']
	})

	export class CharacterListComponent { }
	```

	and then open `app/pages/characters/characters.html` and paste:

	``` html
	<!-- Action Bar -->
	<ActionBar title="My Characters List">
		<ActionItem ios.systemIcon="4" android.systemIcon="ic_menu_add" ios.position="right" android.position="right"></ActionItem>
	</ActionBar>

	<StackLayout orientation="vertical">
		<!-- Characters List -->
		<GridLayout>
			<ListView>
				<template>
					<GridLayout class="fontAwesome-icons padding-m" columns="100,*,30" rows="*" verticalAlignment="center">
						<!-- Thumbnail -->
						<Image col="0" row="0" verticalAlignment="center" class="margin-m"></Image>

						<!-- Inputs -->
						<StackLayout orientation="vertical" col="1" row="0" verticalAlignment="center">
								<Label verticalAlignment="center" text="Insert name" > </Label>
								<Label verticalAlignment="center" textWrap="true" text="Insert Description" class="character-desc"></Label>
						</StackLayout>

						<!-- Icon -->
						<Label text="&#xf014;" col="2" row="0" verticalAlignment="center" class="trashIcon"></Label>
					</GridLayout>
				</template>
			</ListView>
		</GridLayout>
	</StackLayout>
	```

18. Now, to be able to show our new characters component let's add some action to the login button. Open your `app/app.routing.ts` file and paste:

	``` javascript
	import { LoginComponent } from "./pages/login/login";
	import { CharacterListComponent } from "./pages/characters/characters";

	export const routes = [
		{ path: "", component: LoginComponent },
		{ path: "characters", component: CharacterListComponent }
	];

	export const navigatableComponents = [
		LoginComponent,
		CharacterListComponent
	]; 
	```

	now let's go and add the tap event logic for our login button, copy and paste the following code inside your `app/pages/login/login.ts` file:
	
	``` javascript
	import {Component} from "@angular/core";
	import { Router } from "@angular/router";
	import {Page} from "ui/page";

	@Component({
		selector: "my-app",
		templateUrl: "pages/login/login.html",
		styleUrls: ['pages/login/login-common.css', 'pages/login/login.css']
	})

	export class LoginComponent {
		public email: string;
		public password: any;

		constructor(private router: Router, page: Page) {
			page.actionBarHidden = true;
		}

		public login(): void {
			this.router.navigate(["/characters"]);
		}
	}
	```

	and then let's add a tap event handler for our login html, copy and paste the following code inside your `app/pages/login/login.html`
	
	``` html
	<GridLayout rows="*,*,*,*" columns="*" verticalAlignment="center" horizontalAlignment="stretch">
		<!-- Text -->
		<Label row="0" col="0" text="Login" textWrap="true" class="login-title"></Label>

		<!-- Email Input -->
		<AbsoluteLayout row="1" col="0" class="fontAwesome-icons">
			<Label text="&#xf007;"></Label>
			<TextField hint="Email Address" keyboardType="email" [(ngModel)]="email" autocorrect="false" autocapitalizationType="none"></TextField>
		</AbsoluteLayout>

		<!-- Password Input -->
		<AbsoluteLayout row="2" col="0" class="fontAwesome-icons margin-bottom-m">
			<Label text="&#xf023;"></Label>
			<TextField [(ngModel)]="password" hint="Password" secure="true"></TextField>
		</AbsoluteLayout>

		<!-- Login Btn -->
		<Button row="3" col="0" text="Ingresar" (tap)="login()" class="btn-fontSize btn-positive"></Button>
	</GridLayout>
	```

19. Now we are inside our app but there's nothing there yet, so, let's add some lists in it. First we'll add a model for our list Characters. Open your `app/model/character-model.ts` file and paste:

	``` javascript
	export class Character {
		imageUrl: string;
		name: string;
		description: any;
	}

	export class Data {
		CHARACTERS: Character[] = [
					{ name: "TJ-VanToll", description: "Telerik developer. Nowadays helps web developers build mobile apps through projects like NativeScript", imageUrl: "~/images/TJ-VanToll.jpeg" },
					{ name: "Burke Holland", description: "Burke works for Telerik as a Developer Advocate focusing on Kendo UI.", imageUrl: "~/images/burke-holland.png" },
					{ name: "John Papa", description: "Google Developer Expert, Microsoft Regional Director and MVP, author of 100+ articles and 10 books", imageUrl: "~/images/john-papa.png" },
					{ name: "Brendan Eich", description: "American technologist and creator of the JavaScript programming language", imageUrl: "~/images/brendan-eich.jpg" }
			];
	}
	```

20. Now, let's create a service that will handle the data and every action we'll trigger on it, open your `app/services/character.service.ts` file and paste:

	``` javascript
	import {Injectable} from "@angular/core";
	import { Character, Data } from '../model/character-model';

	@Injectable()
	export class CharactersService {
		public listCharacters;

		public getCharacters() {
			this.listCharacters = new Data();
			return this.listCharacters.CHARACTERS;
		}

		public removeCharacter(item: Character): void {
			this.listCharacters.CHARACTERS.splice(this.listCharacters.CHARACTERS.indexOf(item), 1);
		}

		public addCharacter(item: Character): void {
			this.listCharacters.CHARACTERS.push(item);
		}
	}
	```

21. Now, let's call our service inside our Characters component, open your `app/pages/characters/characters.ts` file and paste:

	``` javascript
	import {Component, ElementRef, OnInit, ViewChild} from "@angular/core";
	import * as application from "application";
	import * as platform from "platform";
	import {CharactersService} from '../../services/characters.service';
	import {Character} from "../../model/character-model";

	@Component({
		selector: 'characters-list',
		templateUrl: 'pages/characters/characters.html',
		styleUrls: ['pages/characters/characters.css'],
		providers: [CharactersService, Character]
	})

	export class CharacterListComponent {
		public characterList;
		public switchCharacter: boolean = false;

		constructor(private charactersService: CharactersService, private character: Character) {
		}

		public ngOnInit(): void {
			this.characterList = this.charactersService.getCharacters();
		}

		public removeCharacter(item): void {
			this.charactersService.removeCharacter(item);
		}
	}
	```

22. To see our list working let's bind the model to the Characters.html file, open your `app/pages/characters/characters.html` file and paste:

	``` html
	<!-- Action Bar -->
	<ActionBar title="My Characters List">
		<ActionItem ios.systemIcon="4" android.systemIcon="ic_menu_add" ios.position="right" android.position="right" ></ActionItem>
	</ActionBar>

	<StackLayout orientation="vertical">
		<!-- Characters List -->
		<GridLayout>
			<ListView [items]="characterList">
				<template let-item="item">
					<GridLayout class="fontAwesome-icons padding-m" columns="100,*,30" rows="*" verticalAlignment="center">
						<!-- Thumbnail -->
						<Image [src]="item.imageUrl" col="0" row="0" verticalAlignment="center" class="margin-m"></Image>

						<!-- Inputs -->
						<StackLayout orientation="vertical" col="1" row="0" verticalAlignment="center">
							<Label verticalAlignment="center" [text]="item.name" > </Label>
							<Label verticalAlignment="center" textWrap="true" [text]="item.description" class="character-desc"></Label>
						</StackLayout>
						
						<!-- Icon -->
						<Label text="&#xf014;" col="2" row="0" verticalAlignment="center" class="trashIcon" (tap)="removeCharacter(item)"></Label>
					</GridLayout>
				</template>
			</ListView>
		</GridLayout>
	</StackLayout>
	```

	hmmm... it seems our list could look prettier, so let's add some styling, open your `app/pages/characters/characters.scss` file and paste:

	``` scss
	@import 'styles/_variables.scss';

	.addIcon {
		width: 50;
		text-align: center;
	}

	.trashIcon {
		color: $red;
	}

	.addIcon-padding {
		padding-right: 0;
	}

	.character-name {
		font-size: 16;
		font-weight: bold;
	}

	.character-desc {
		margin-top: 5;
		font-size: 15;
		padding-right: 15;
	}

	.btn-height {
		height: 50;
	}

	.btn-height-m {
		height: 70;
	}
	```

23. Now let's do some hardware access. We'll have to create a new character and obviously upload a picture to it but to do that first we'll have to add some Nativescript plugins. If you are still listening the live changes, type **Ctrl+C** in your terminal to kill it. Go to your terminal and paste:

	``` sh
	tns plugin add nativescript-imagepicker
	tns plugin add nativescript-permissions
	```
	>PLease note that for IOS 10 and newer versions you need to rquest for some permissions before access the user private data like gallery, contact number, photos , location , calendar , etc. So you'll have to declare the following in your `app/App_Resources/iOS/Info.plist` file:
	
	>``` xml
	><key>NSPhotoLibraryUsageDescription</key>
	><string>${PRODUCT_NAME} photo use</string>
	><key>NSCameraUsageDescription</key>
	><string>${PRODUCT_NAME} camera use</string>
	>```
	and then let's listen our changes again:

	``` sh
	tns livesync android --emulator --watch / tns livesync android --watch 
	tns livesync ios --emulator --watch / tns livesync ios --watch
	```

24. Now let's add some functions that will create our new Character and will handle the events to upload the image from the gallery, open your `app/pages/characters/characters.ts` file and paste:

	``` javascript
	import {Component, ElementRef, OnInit, ViewChild} from "@angular/core";
	import * as application from "application";
	import * as platform from "platform";
	import {CharactersService} from '../../services/characters.service';
	import {Character} from "../../model/character-model";

	var imagepickerModule = require("nativescript-imagepicker");
	var permissions = require("nativescript-permissions");
	declare var android: any;

	@Component({
		selector: 'characters-list',
		templateUrl: 'pages/characters/characters.html',
		styleUrls: ['pages/characters/characters.css'],
		providers: [CharactersService, Character]
	})

	export class CharacterListComponent {
		public characterList;
		public switchCharacter: boolean = false;

		constructor(private charactersService: CharactersService, private character: Character) {
		}

		public ngOnInit(): void {
			this.characterList = this.charactersService.getCharacters();
		}

		public cancel(): void {
			this.switchCharacter = false;
		}

		public addNewCharacter(): void {
			this.character = new Character();
			this.character.imageUrl = "~/images/camera-icon.png";
			this.switchCharacter = true;
		}

		public save(item): void {
			this.charactersService.addCharacter(item);
			this.switchCharacter = false;
		}

		public removeCharacter(item): void {
			this.charactersService.removeCharacter(item);
		}


		public uploadImage(): void {
			let context = imagepickerModule.create({
				mode: "single"
			});
			if (platform.device.os === "Android" && platform.device.sdkVersion >= "23") {
				permissions.requestPermission(android.Manifest.permission.READ_EXTERNAL_STORAGE, "I need these permissions to read from storage")
					.then(() => {
						this.startSelection(context);
					})
					.catch((err) => {
						console.log("Permiso denegado! :( ", err);
					});
			} else {
				this.startSelection(context);
			}
		}

		public startSelection(context): void {
			context
				.authorize()
				.then(() => {
					return context.present();
				})
				.then((selection) => {
					selection.forEach((selected) => {
						this.character.imageUrl = selected.thumb;
					});
				})
				.catch((e) => {
					console.log(e);
				});
		}
	}
	```

25. And last, let's add the elements that will add our new character to the list, open your `app/pages/characters/characters.html` file and paste:

  ``` html
	<!-- Action Bar -->
	<ActionBar title="My Characters List">
		<ActionItem ios.systemIcon="4" android.systemIcon="ic_menu_add" ios.position="right" android.position="right" (tap)="addNewCharacter()"></ActionItem>
	</ActionBar>

    <StackLayout orientation="vertical">
	    <!-- Ann New CHaracter -->
      <GridLayout  columns="80,*" rows="100,*,*" verticalAlignment="center" *ngIf="switchCharacter" class="padding-m">
  	    <!-- Thumbnail -->
        <Image col="0" rowSpan="2" verticalAlignment="center" [src]="character.imageUrl" (tap)="uploadImage()" class="margin-m"></Image>
    
				<!-- Inputs -->
        <StackLayout orientation="vertical" col="1" rowSpan="2" verticalAlignment="center">
        	<TextField hint="Enter name" keyboardType="text" autocorrect="false" autocapitalizationType="none" [(ngModel)]="character.name" class="margin-bottom-s btn-height"></TextField>
          <TextView hint="Enter description" [editable]="true" [(ngModel)]="character.description" class="btn-height-m"></TextView>
        </StackLayout>

        <!-- Buttons -->
        <GridLayout rows="*" columns="*, *" colSpan="2" row="2" verticalAlignment="center" class="margin-noTop-m">
        	<Button row="0" col="0" text="Save" (tap)="save(character)" class="btn-fontSize btn-positive btn-height"></Button>
          <Button row="0" col="1" text="Cancel" (tap)="cancel()" class="btn-fontSize btn-green margin-left-s btn-height"></Button>
      	</GridLayout>
      </GridLayout>

        <!-- Characters List -->
        <GridLayout>
        	<ListView [items]="characterList">
          	<template let-item="item">
            	<GridLayout class="fontAwesome-icons padding-m" columns="100,*,30" rows="*" verticalAlignment="center">
	              <!-- Thumbnail -->
              	<Image [src]="item.imageUrl" col="0" row="0" verticalAlignment="center" class="margin-m"></Image>
                <!-- Inputs -->
  							<StackLayout orientation="vertical" col="1" row="0" verticalAlignment="center">
                	<Label verticalAlignment="center" [text]="item.name" > </Label>
                  <Label verticalAlignment="center" textWrap="true" [text]="item.description" class="character-desc"></Label>
                </StackLayout>

                <!-- Icon -->
                <Label text="&#xf014;" col="2" row="0" verticalAlignment="center" class="trashIcon" (tap)="removeCharacter(item)"></Label>
              </GridLayout>
            </template>
         </ListView>
      </GridLayout>
    </StackLayout>
    ```

26. And that's it, we have our Character list working!!!
