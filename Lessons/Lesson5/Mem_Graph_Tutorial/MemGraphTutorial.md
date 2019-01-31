# Using the Debug Memory Graph Tool

### Identify A Memory Leaky

1. ensure that the active scheme is set to Debug:

<!-- TODO: screen shot here -->

2. Run the LeakyStarship starter app and click the button two or three times…

3. In the Debug area, click the Debug Memory Graph button on the Debug Toolbar:

<!-- TODO: insert screenshot of button on tool bar here -->


Let’s observe what occurs in Xcode:
- on click of the Debug Memory Graph button, Xcode sets a temporary (system) breakpoint in your app at the point where you clicked the button
- the Navigator panel switches to the Debug Navigator and displays a graph of the current objects in memory


<!-- TODO: screen shot here -->

***Note:*** *all the purple squares with exclamation points indicate memory leaks.*

<!-- TODO: screen shot here -->

4. Now, let’s focus the Debug Navigator on only those objects in memory causing memory leaks:
at the bottom right of the Navigator pane, click the little rectangular “Show only leaked blocks” icon

<!-- TODO: screen shot here -->

- Examine the leaks found under the Captain, CrewMember and Starship objects.
- Also, look briefly at to the memory leaks under two ContiguousArrayStorage objects.

### Finding the Source of the Leak

Let’s examine the source of one of the memory leaks caused by strong reference cycles.

For now, let’s focus only on the leaks in CrewMember objects.

5. In the Debug Navigator panel, select the CrewMember object you want to examine. If open, close the Assistant Editor and minimize the Debug area to maximize your view of your CrewMember object’s memory graph in Xcode’s middle panel.

<!-- TODO: screen shot here -->

- Notice that each of the CrewMember objects holds a strong reference to the Captain object — while the Captain object holds a strong reference to each of the CrewMember objects.

- Notice, too, that the Captain object (cube) holds a strong reference to its CrewMember objects through the array object, `_ContiguousArrayStorage<CrewMember>` (by default, arrays in Swift hold strong references to their elements).

### Fixing The Leak

6. Ctrl+Click (or Right+Click) one of the CrewMember object cubes in the memory graph.

7. The context menu displayed offers several useful choices. Choose the Jump to Definition selection in the dropdown list.

Xcode should open your CrewMember class file:

```Swift
class CrewMember
{
    let name:String
    var captain:Captain

    init(name: String, captain: Captain)
    {
        self.name = name
        self.captain = captain

        print("CrewMember \(name) instance allocated.\n")
    }

    deinit
    {
        print("CrewMember \(name) instance deallocated.\n")
    }
}
```

**Q:** Based on what you’ve learned so far about strong reference cycles, how would you fix the memory leak currently created for the CrewMember objects.

**TODO:**
- Apply your solution
- Run the app again and bring up the Memory Graph Debug tool.

**RESULTS:**
1. In addition to adding the *weak* keyword, what other change was required?
- why?
2. What happens differently in your Memory Graph Debugger now?
(hint: How many objects were effected?)
- why?
3. Is anything different for either of the 2 ContiguousArrayStorage objects?
- If there has been a change to those objects, why did that change occur?
