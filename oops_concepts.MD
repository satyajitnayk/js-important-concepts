**Class:** A class is a blueprint or a template that defines the structure and behavior of objects. It defines the properties (attributes) and methods (functions) that objects of that class will have.

**Object:** An object is an instance of a class. It represents a real-world entity with its own unique state and behavior.
An Album and a Track class, and another file that plays a track.

```typescript
export class Album {
  name: string;

  constructor(name: string) {
    this.name = name;
  }
}

export class Track {
  name: string;
  length: number;
  album: Album;

  constructor(name: string, length: number, album: Album) {
    this.name = name;
    this.length = length;
    this.album = album;
  }
}

// meanwhile in another file ...

play() {
  // Finds the track
  // Play it
}
```

We will start to apply the OOP concepts, but we'll keep it simple, especially because the goal here is to understand
those concepts and not to build a real audio streaming API.

1. **Encapsulation**

- All those properties in the Track class are accessible by anyone, let's change that by encapsulating them.
  Encapsulation
  is a mechanism to hide information, those information are the classes members, like properties and methods, this
  is
  achieved by using the modifiers public, protected, and private.

- public members can be accessed anywhere, this is the default modifier.
  protected members are visible inside of the class they’re declared in and its subclasses.
- private members are only visible inside of the class they’re declared in.
  To encapsulate all those properties we follow these steps:
- Add modifier private :Add _ before the property name, for example, _name
- Implement the accessors get and set. These methods will be invoked whenever you try to get o set the property
  value.
- Making all properties private the Track class will look like this:

  ```typescript
  export class Track {
    private _name: string;
    private _length: number;
    private _album: Album;
        
    constructor(name: string, length: number, album: Album) {
        this._name = name;
        this._length = length;
        this._album = album;
    }
        
    get name(): string {
        return this._name;
    }
        
    set name(name: string) {
        this._name = name;
    }
        
    get length(): number {
        return this._length;
    }
        
    set length(length: number) {
        this._length = length;
    }
        
    get album(): Album {
        return this._album;
    }
        
    set album(album: Album) {
        this._album = album;
    }
  }
  ```

2. **Abstraction**

- Abstraction is about hiding complexity details from the user, for example, to drive a car you don't need to know how
  its
  engine works, so in a car it's abstracted and an interface (steering wheel, pedals, gear shift, etc.) is provided to
  you, and you only need to know how to operate that interface.

- Take a look at how a track is being played, it first finds the track, and then plays it. Seems simple, now suppose we
  have to do the same all over places whenever we need to play a track, that would be redundant, but there are other
  problems, everyone who is writing a code to play a track need to know how to do it, and what if something in playing
  track changes? We would have to change it in many places.

- This is an opportunity to abstract that logic, we do that by moving it to inside the track class, and now whenever a
  track needs to be played, anyone only has to call that play method without knowing its details:

    ```typescript
    export class Track {
        private _name: string;
        private _length: number;
        private _album: Album;
        
        constructor(name: string, length: number, album: Album) {
            this._name = name;
            this._length = length;
            this._album = album;
        }
        
        get name(): string {
            return this._name;
        }
        
        set name(name: string) {
            this._name = name;
        }
        
        get length(): number {
            return this._length;
        }
        
        set length(length: number) {
            this._length = length;
        }
        
        get album(): Album {
            return this._album;
        }
        
        set album(album: Album) {
            this._album = album;
        }
    
        play() {
            // Play it
        }
    }
    
    // To instantiate and play a track
    const album = new Album("Rocks");
    const track = new Track("Combination", 264, album);
    
    track.play();
    
    ```

3. **Inheritance**

- Suppose that now our audio streaming software needs to add support to podcasts, and these podcasts have episodes, how
  would we do that? We could simply add a Podcast class, and a type property in track class:

    ```typescript
    export class Podcast {}
    
    export class Track {
        private _name: string;
        private _length: number;
        private _type: string; // track or episode
        private _album?: Album;
        private _podcast?: Podcast;
    
        ...
    
    }
    ```

- That works but it is more difficult to maintain, we have to consider the type in this class behaviors, like in the
  play
  method, and if we have to support another type of media, we would have to modify everything again.
- So, how can we solve this problem? Enters the inheritance: A mechanism that allows us to create class hierarchies,
  where
  a subclass can inherit properties and behaviors from its superclass. This is great for reusability.

- So, the solution suggested is the following:

- First, create an Audio class, with the properties and behaviors that are common for tracks and episodes, this will be
  our superclass:

    ```typescript
    export class Audio {
        private _name: string;
        private _length: number;
        
        constructor(name: string, length: number) {
            this._name = name;
            this._length = length;
        }
    
        /** getters and setters */
    
        play() {
            // Play it
        }
    
    }
    ```

- Then, the Track class will derive from the Audio class, meaning it will inherit the Audio class properties and
  behaviors:

    ```typescript
    export class Track extends Audio {
        private _album: Album;
        
        constructor(name: string, length: number, album: Album) {
            super(name, length);
            this._album = album;
        }
        
        get album(): Album {
            return this._album;
        }
        
        set album(album: Album) {
            this._album = album;
        }
    }
    ```

- The extends keyword means that the Track class is inheriting from the Audio class, and since Track is extending
  another
  class, we have to call super in the constructor, even if there are not any properties to pass to the superclass. Also,
  super can be used to call the parent class methods.
  ```typescript
    class Animal {
      constructor(public name: string) {}
    
      makeSound() {
        console.log("Generic animal sound");
      }
    }
    
    class Dog extends Animal {
      constructor(name: string) {
        super(name); // Call the constructor of the parent class.
      }
    
      makeSound() {
        super.makeSound(); // Call the makeSound method from the parent class.
        console.log("Bark!"); // Add additional behavior in the child class.
      }
    }
    
    const dog = new Dog("Buddy");
    dog.makeSound();

  ```
- It's worthy to note that a superclass can be called a parent class, and a subclass can also be called a child class.

- Finally, we create the Podcast and Episode classes, Episode will extend Audio class too:

    ```typescript
    export class Podcast {
        name: string;
        
        constructor(name: string) {
            this.name = name;
        }
    }
    
    export class Episode extends Audio {
        private _podcast: Podcast;
        
        constructor(name: string, length: number, podcast: Podcast) {
            super(name, length);
            this._podcast = podcast;
        }
        
        get podcast(): Podcast {
            return this._podcast;
        }
        
        set podcast(podcast: Podcast) {
            this._podcast = podcast;
        }
    }
    ```

4. **Polymorphism**

- Now suppose that for some reason, when users close the streamer and open it again, only podcasts episodes need to play
  where they left off, and tracks have to start from the beginning. As you know the play method is in the Audio class,
  so
  it is the same for both episodes and tracks, how do we implement this feature? Polymorphism comes to the rescue!

- Polymorphism is the idea of an object can take multiple forms as the word suggests, we'll use it to make the play
  method
  perform differently for tracks and episodes. In practice, we do that by overriding it in the Episode class, which
  means
  we implement the play method in the Episode class differently from how it is implemented in the superclass:

   ```typescript
    export class Episode extends Audio {
    
        /** everything else remains the same */
    
        play() {
          // Check where it left off
          // Play it
        }
    
    }
    ```

5. **Interface**

- In simpler word we can say `interface` defines _what to implement_ but it depends on the class that extends it on how
  to implement.
- Suppose you have different types of audio files, such as tracks and podcast episodes, that need to be played by your
  media player. You have classes like `Track` and `Episode` that represent these audio files. Here's an example:

    ```typescript
    class Track {
      private _name: string;
      private _length: number;
    
      constructor(name: string, length: number) {
        this._name = name;
        this._length = length;
      }
    
      // Other methods specific to Track
      // ...
    }
    
    class Episode {
      private _name: string;
      private _length: number;
    
      constructor(name: string, length: number) {
        this._name = name;
        this._length = length;
      }
    
      // Other methods specific to Episode
      // ...
    }
    ```

  Now, let's say you want to create a media player class that can play both tracks and episodes. Without interfaces, you
  might be tempted to write code like this:

    ```typescript
    class MediaPlayer {
      playTrack(track: Track) {
        // Logic to play a track
      }
    
      playEpisode(episode: Episode) {
        // Logic to play an episode
      }
    }
    ```

  The problem with this approach is that the `MediaPlayer` class is tightly coupled to the `Track` and `Episode`
  classes.
  If you introduce a new type of audio file in the future, you would need to modify the `MediaPlayer` class to handle
  it.
  This violates the Open-Closed Principle, which states that classes should be open for extension but closed for
  modification.

  To solve this, you can introduce an interface, let's call it `Playable`, that defines the contract for any audio file
  that can be played. Here's an example:

    ```typescript
    interface Playable {
      name: string;
      length: number;
    
      play(): void;
    }
    ```

  Now, you can update the `Track` and `Episode` classes to implement the `Playable` interface:

    ```typescript
    class Track implements Playable {
      private _name: string;
      private _length: number;
    
      constructor(name: string, length: number) {
        this._name = name;
        this._length = length;
      }
    
      // Implement the Playable interface
      get name(): string {
        return this._name;
      }
    
      get length(): number {
        return this._length;
      }
    
      play(): void {
        // Logic to play a track
      }
    }
    
    class Episode implements Playable {
      private _name: string;
      private _length: number;
    
      constructor(name: string, length: number) {
        this._name = name;
        this._length = length;
      }
    
      // Implement the Playable interface
      get name(): string {
        return this._name;
      }
    
      get length(): number {
        return this._length;
      }
    
      play(): void {
        // Logic to play an episode
      }
    }
    ```

  With the interface in place, you can now update the `MediaPlayer` class to accept any object that implements
  the `Playable` interface:

    ```typescript
    class MediaPlayer {
      playMedia(media: Playable) {
        // Logic to play the media
      }
    }
    ```

- By programming against the `Playable` interface, you achieve several benefits:

    1. **Code Organization**: The `MediaPlayer` class doesn't need to know about specific implementations like `Track`
       or `Episode`. It only cares about objects that conform to the `Playable` interface, which makes the code more
       modular
       and maintainable.

    2. **Polymorphism**: The `MediaPlayer` class can now handle any object that implements the `Playable` interface.
       This
       promotes code reusability and flexibility. You can introduce new types of playable media in the future without
       modifying the `MediaPlayer` class.

    3. **Type Safety**: The compiler can enforce that any object passed to the `MediaPlayer` class's `playMedia` method
       must
       implement the `Playable` interface. This helps catch errors at compile-time and ensures that the required
       properties
       and methods are available.

- Overall, using interfaces allows for loose coupling between different parts of the code, promotes polymorphism, and
  ensures type safety. It enables you to write more modular, extensible, and maintainable code.

6. **Abstract Class**

- **Need even if we have Interface:** interfaces can also be used to enforce contracts and provide a common interface
  for classes to implement. While
  abstract classes can define both property and method implementations, interfaces focus solely on defining the shape of
  an object or class without providing any default implementation.
- Abstract class helps us to enforce contracts and can define both property and method implementations .
- Can not be instantiated.

    ```typescript
    abstract class Media {
      constructor(public title: string, public duration: number) {
      }
      // method is abstract so child class has to implement the function    
      abstract play(): void;
    }
    
    class Track extends Media {
      play(): void {
        console.log(`Playing track: ${this.title}`);
      }
    }
    
    class Episode extends Media {
      constructor(title: string, duration: number, public episodeNumber: number) {
        super(title, duration);
      }
    
      play(): void {
        console.log(`Playing episode ${this.episodeNumber}: ${this.title}`);
      }
    }
    
    const track1 = new Track('Song 1', 180);
    track1.play();  // Output: Playing track: Song 1
    
    const episode1 = new Episode('Episode 1', 2400, 1);
    episode1.play();  // Output: Playing episode 1: Episode 1
    
    ```
