# 第21回: １時間でiPhoneアプリを作ろう
## 花火アプリ

  <div style="text-align:center"><img src ="https://github.com/iosClassForBeginner/fireworks-jp/blob/master/samples/sample.gif" width="50%" height="50%"/></div>  

  当アカウントへ訪れていただき、誠にありがとうございます。第21回アプリ教室では、花火アプリを作ります。自分のペースで勉強したい、勉強前に予習したい、内容を復習したい場合、ご利用ください。

## アプリ教室に興味ある方、歓迎します。  
  Meetup  
  http://www.meetup.com/ios-dev-in-namba/
  
## 別途アプリ教室(有料)も開いております  
  http://learning-ios-dev.esy.es/  

## 問い合わせ
  株式会社ジーライブ
  http://geelive-inc.com  

## 開発環境
  Xcode 8.3.2 / Swift 3
  
  ・<a href="https://github.com/learn-co-students/reading-ios-intro-to-xcode-qa-public-001">What is Xcode?</a>

## アプリ作成手順
## 0, プロジェクト作成

> 0-1. Xcodeを起動。
>
> 0-2. "Create a new Xcode project"を選択。
>
> 0-3. "Single View Application"を選択して"Next"をクリック。
>
> 0-4. "Product name"を適当に入力して"Next"をクリック。
>
> 0-5. プロジェクトの場所を指定して"Create"をクリック。

## 1, 画像素材を追加する

> 1-1. 画像素材を"Assets.xcassets"にドラッグ&ドロップ。
<details>
<summary>詳細画像をみる</summary>
<div style="text-align:center"><img src ="https://github.com/iosClassForBeginner/fireworks-jp/blob/master/samples/0.gif" /></div>
</details>

[画像ファイルをダウンロード](https://github.com/iosClassForBeginner/fireworks-jp/raw/master/images/Images.zip)

## 2, アプリをデザインする。
#### 🗂 Main.storyboard

> 2-0. 背景を黒に変更
> <details><summary>詳細画像をみる</summary><div style="text-align:center"><img src ="https://github.com/iosClassForBeginner/fireworks-jp/blob/master/samples/2.gif" /></div></details>

> 2-1. "UIImageView"を設置
> <details><summary>詳細画像をみる</summary><div style="text-align:center"><img src ="https://github.com/iosClassForBeginner/fireworks-jp/blob/master/samples/3.gif" /></div></details>

> 2-2. "UIImageView"の大きさ変更
> <details><summary>詳細画像をみる</summary><div style="text-align:center"><img src ="https://github.com/iosClassForBeginner/fireworks-jp/blob/master/samples/4.gif" /></div></details>
> <details><summary>詳細画像をみる</summary><div style="text-align:center"><img src ="https://github.com/iosClassForBeginner/fireworks-jp/blob/master/samples/5.gif" /></div></details>
> <details><summary>詳細画像をみる</summary><div style="text-align:center"><img src ="https://github.com/iosClassForBeginner/fireworks-jp/blob/master/samples/6.gif" /></div></details>

## 3, 下記のコードを"ViewController.swift"に追加
#### 🗂 ViewController.swift

参考ドキュメント
・<a href="https://developer.apple.com/documentation/quartzcore/caemitterlayer">CAEmitterLayer</a> 
・<a href="https://developer.apple.com/documentation/quartzcore/caemittercell">CAEmitterCell</a>

```Swift  
import UIKit

class ViewController: UIViewController
{
    let emitterLayer = CAEmitterLayer()
    
    override func viewDidLoad()
    {
        super.viewDidLoad()
        setupBaseLayer()
        launchFireworks()
    }
    
    func setupBaseLayer()
    {
        let size = view.bounds.size
        emitterLayer.emitterPosition = CGPoint(x: size.width / 2, y: size.height - 100)
        emitterLayer.renderMode = kCAEmitterLayerAdditive
        view.layer.addSublayer(emitterLayer)
    }
    
    func launchFireworks()
    {
        // パーティクル画像を取得
        let particleImage = UIImage(named: "particle")?.cgImage
        
        // パーティクルを定義する(花火開始)
        let baseCell = CAEmitterCell()
        baseCell.color = UIColor.white.withAlphaComponent(0.8).cgColor
        baseCell.emissionLongitude = -CGFloat.pi / 2
        baseCell.emissionRange = CGFloat.pi / 5
        baseCell.emissionLatitude = 0
        baseCell.lifetime = 2.0
        baseCell.birthRate = 1
        baseCell.velocity = 400
        baseCell.velocityRange = 50
        baseCell.yAcceleration = 300
        baseCell.redRange   = 0.5
        baseCell.greenRange = 0.5
        baseCell.blueRange  = 0.5
        baseCell.alphaRange = 0.5
        
        // パーティクルを定義する(花火打ち上げ中)
        let risingCell = CAEmitterCell()
        risingCell.contents = particleImage
        risingCell.emissionLongitude = (4 * CGFloat.pi) / 2
        risingCell.emissionRange = CGFloat.pi / 7
        risingCell.scale = 0.4
        risingCell.velocity = 100
        risingCell.birthRate = 50
        risingCell.lifetime = 1.5
        risingCell.yAcceleration = 350
        risingCell.alphaSpeed = -0.7
        risingCell.scaleSpeed = -0.1
        risingCell.scaleRange = 0.1
        risingCell.beginTime = 0.01
        risingCell.duration = 0.7
        
        // パーティクルを定義する(花火爆発)
        let sparkCell = CAEmitterCell()
        sparkCell.contents = particleImage
        sparkCell.emissionRange = 2 * CGFloat.pi
        sparkCell.birthRate = 8000
        sparkCell.scale = 0.5
        sparkCell.velocity = 130
        sparkCell.lifetime = 3.0
        sparkCell.yAcceleration = 80
        sparkCell.beginTime = 1.5
        sparkCell.duration = 0.1
        sparkCell.alphaSpeed = -0.1
        sparkCell.scaleSpeed = -0.1
        
        // パーティクルの定義を一つにまとめる
        baseCell.emitterCells = [risingCell, sparkCell]
        
        // レイヤーにパーティクル追加
        emitterLayer.emitterCells = [baseCell]
    }
    
}
```
