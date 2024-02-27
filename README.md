# Vision Pro Programming
Some tips when programming for VisionOS


## Gaze highlighting

To apply a HoverEffectComponent to an entity in RealityKit, you should first ensure that your entity is prepared to use components effectively. In a practical example from the Apple Developer Forums, adding a hover effect to a default sphere in a visionOS Volume project involves setting the HoverEffectComponent directly on the entity after it's been identified within the scene. This is done by using the components.set method on the entity, as shown in a simple code snippet:

```
if let sphere = scene.findEntity(named: "Sphere") {
    sphere.components.set(HoverEffectComponent())
    content.add(sphere)
}
```

## Applying material to a sphere primitive

For my Altair simulator, I want an "LED" to change from dull to bright red.

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
