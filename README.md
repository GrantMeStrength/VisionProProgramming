# Vision Pro Programming
Some tips when programming for VisionOS


## Gaze highlighting

To make an Entity change appearance when the user looks at it, use the HoverEffectComponent. Adding a hover effect to a default sphere in a visionOS Volume project involves setting the HoverEffectComponent directly on the entity after it's been identified within the scene. This is done by using the components.set method on the entity, as shown:

```
if let sphere = scene.findEntity(named: "Sphere") {
    sphere.components.set(HoverEffectComponent())
    content.add(sphere)
}
```

The Entity needs to have targeting and collision components set (either in code or in the Reality Editor) before this works.

## Applying material to a sphere primitive

For my Altair 8800 simulator, I want an "LED" to change from dull to bright red.

### Creating a simple material

```
@State private var led_on =  SimpleMaterial(color: UIColor(red: 2.5, green: 0, blue: 0, alpha: 1), roughness: 0.0, isMetallic: false)
```

### Applying the material

```
// Note: this works for a sphere primitive, but fails on other primitives e.g. capsule

fileprivate func applyMaterial(_ led: Entity, _ stuff: SimpleMaterial) {
        var modelComponent = led.components[ModelComponent.self]!
        modelComponent.materials = [stuff]
        led.components.set(modelComponent)
    }

applyMaterial(myEntity, led_on)
```

## Reacting to an Entity being tapped and dragged

The Entity needs to have targeting and collision components set (either in code or in the Reality Editor) before this works.

```
var body: some View {
        RealityView { content in
            // Add the initial RealityKit content
            if let scene = try? await Entity(named: "Scene", in: realityKitContentBundle) {
                content.add(scene)

                // Scene stuff

            }
        }
    }

      // This is "clicking" on an object.
     .gesture(TapGesture().targetedToAnyEntity().onEnded { value in

        if value == "MyEntityName" {
            print("Tapped!")
        }
    })

    // This dragging. Can be used with tapping too.
    // This is the 'minimumDistance' version, there is a version without this requirement for immediately feedback
     .gesture(DragGesture(minimumDistance: 20).targetedToAnyEntity().onChanged { value in
           
            let direction_is_down = (value.gestureValue.translation.height) > 0

            if direction_is_down {
                print("Entity dragged down after a little distance to make sure that's the intention")
            }

        }
    })


```

                
