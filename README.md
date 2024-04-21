# SceneKit-Mesh-generation-Octahedral

# The first Part
![IMG_2039](https://github.com/S-way520/SceneKit-Mesh-generation-Octahedral/assets/95877651/1e6a3cd4-9515-417c-9713-e832c9abd46f)

# The Second Part
Code file 1
```swift
import SwiftUI
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```
Code file 2
```swift
import SwiftUI
import SceneKit

struct ContentView: View {
    @State private var scene: SCNScene?
    @State var shapeColor: CGColor = UIColor.blue.cgColor
    
    var body: some View {
        SceneKitView(scene: $scene)
            .onAppear {
                generate()
            }
    }
    func generate() {
        let scene = SCNScene()
        
        let material = SCNMaterial()
        material.diffuse.contents = shapeColor
        
        let Geometry = Geometry.Octahedron()
        Geometry.materials = [material]
        
        let Node = SCNNode()
        Node.geometry = Geometry
        
        scene.rootNode.addChildNode(Node)
        self.scene = scene
    }
}
```
Code file 3
```swift
import SwiftUI
import SceneKit

struct SceneKitView: UIViewRepresentable {
    @Binding var scene: SCNScene?
    
    func makeUIView(context: Context) -> SCNView {
        let sceneView = SCNView()
        sceneView.backgroundColor = UIColor.systemBackground
        sceneView.autoenablesDefaultLighting = true
        sceneView.allowsCameraControl = true
        sceneView.scene = SCNScene()
        
        let camera = SCNCamera()
        camera.zFar = 100
        camera.zNear = 0.01
        
        let cameraNode = SCNNode()
        cameraNode.camera = camera
        cameraNode.position = SCNVector3(x: 0, y: 0, z: 0.5) 
        cameraNode.eulerAngles = SCNVector3(x: 0, y: 0, z: 0)
        
        sceneView.pointOfView = cameraNode
        
        return sceneView
    }
    func updateUIView(_ uiView: SCNView, context: Context) {
        if let scene = scene {
            uiView.scene?.rootNode.childNodes.forEach { $0.removeFromParentNode() }
            uiView.scene?.rootNode.addChildNode(scene.rootNode)
        }
    }
}
```
Code file 4
```swift
import SwiftUI
import SceneKit

struct Geometry {
    static func Octahedron() -> SCNGeometry {
        //Vertice
        let vertice: [SCNVector3] = [
            SCNVector3(0, 0.1, 0),
            SCNVector3(-0.05, 0, 0.05),
            SCNVector3(0.05, 0, 0.05),
            SCNVector3(0.05, 0, -0.05),
            SCNVector3(-0.05, 0, -0.05),
            SCNVector3(0, -0.1, 0),
        ]
        //Texture
        let textureCoordinate: [CGPoint] = [
            CGPoint(x: 0.5, y: 1.0), 
            CGPoint(x: 0.0, y: 0.5), 
            CGPoint(x: 0.5, y: 0.0), 
            CGPoint(x: 1.0, y: 0.5), 
            CGPoint(x: 0.5, y: 0.5),
            CGPoint(x: 0.5, y: 1.0),
            CGPoint(x: 0.0, y: 0.5),
            CGPoint(x: 0.5, y: 0.0)
        ]
        //Draw
        let indice: [UInt16] = [
            0, 1, 2,
            2, 3, 0,
            3, 4, 0,
            4, 1, 0,
            1, 5, 2,
            2, 5, 3,
            3, 5, 4,
            4, 5, 1
        ]
        
        let source = [
            SCNGeometrySource(vertices: vertice),
            SCNGeometrySource(textureCoordinates: textureCoordinate)
        ]
        let element = SCNGeometryElement(indices: indice, primitiveType: .triangles)
        
        return SCNGeometry(sources: source, elements: [element])
    }
}
```
