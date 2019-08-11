//
//  ViewController.swift
//  Downloader
//
//  Created by Ahmed Salah on 8/11/19.
//  Copyright © 2019 Ahmed Salah. All rights reserved.
//

import UIKit

extension UIColor {
    static var skyBlue: UIColor {
        return UIColor(red: 65 / 255, green: 105 / 255, blue: 225 / 255, alpha: 1.0)
    }
}

struct Animation {
    
    struct Key {
        
        static func position() -> String {
            return "position"
        }
        
        static func transfromScale() -> String {
            return "transform.scale"
        }
        
        static func strokeEnd() -> String {
            return "strokeEnd"
        }
        
        
        static func opacity() -> String {
            return "opacity"
        }
        
    }
    
    struct type {
        
        static func basicAnimation() -> CABasicAnimation {
            let bAnimation = CABasicAnimation(keyPath: Animation.Key.strokeEnd())
            bAnimation.fillMode = CAMediaTimingFillMode.forwards
            bAnimation.isRemovedOnCompletion = false
            return bAnimation
        }
        
        static func pulseAnimation(_ repeatCount: Float = HUGE) -> CASpringAnimation {
            let pAnimation = CASpringAnimation(keyPath: Animation.Key.transfromScale())
            pAnimation.fromValue = 0.0
            pAnimation.toValue = 1.4
            pAnimation.repeatCount = repeatCount
            pAnimation.duration = 0.6
            return pAnimation
        }
        
        static func opacityAnimation() -> CASpringAnimation {
            let oAnimation = CASpringAnimation(keyPath: Animation.Key.opacity())
            oAnimation.fromValue = 0.3
            oAnimation.toValue = 0.1
            oAnimation.repeatCount = HUGE
            oAnimation.duration = 0.5
            return oAnimation
        }
        
        
    }
    
}

class ViewController: UIViewController {
    
    var downloaderTimer = Timer()
    var int_counter: Int = 0

    lazy var backgroundCircleShapeLayer: CAShapeLayer = {
        let shapeLayer = CAShapeLayer()
        shapeLayer.path = UIBezierPath(arcCenter: CGPoint(x: 0, y: 0), radius: self.view.frame.height / 8, startAngle: -CGFloat.pi / 2, endAngle: 270 * CGFloat.pi / 2, clockwise: true).cgPath // Don't Change the Angle Radian Values
        shapeLayer.fillColor = UIColor.skyBlue.cgColor
        shapeLayer.fillMode = CAMediaTimingFillMode.forwards
        shapeLayer.opacity = 0.5
        return shapeLayer
    }()
    
        lazy var foregroundCircleShapeLayer: CAShapeLayer = {
        let shapeLayer = CAShapeLayer()
        shapeLayer.path = UIBezierPath(arcCenter: CGPoint(x: 0, y: 0), radius: self.view.frame.height / 8, startAngle: -CGFloat.pi / 2, endAngle: 270 * CGFloat.pi / 180, clockwise: true).cgPath
        shapeLayer.fillColor = UIColor.black.cgColor
        shapeLayer.strokeColor = UIColor.skyBlue.cgColor
        shapeLayer.lineWidth = 20
        shapeLayer.strokeEnd = 0
        shapeLayer.lineCap = CAShapeLayerLineCap.round
        shapeLayer.fillMode = CAMediaTimingFillMode.forwards
        return shapeLayer
        
    }()
        lazy var downloadLabel: UILabel = {
        let label = UILabel(frame: CGRect(x: 0, y: 0, width: 0150, height: 60))
        label.font = UIFont.boldSystemFont(ofSize: 20)
        label.textColor = .white
        label.text = "0 %"
        label.textAlignment = .center
        label.translatesAutoresizingMaskIntoConstraints = false
        return label
    }()
    
    
    
    
    
    @IBOutlet weak var downloadButton: UIButton!
    
    @IBAction func hasDownloadButtonTapped(_ sender: UIButton) {
        
        downloadButton.isEnabled = false
        foregroundCircleShapeLayer.add(Animation.type.basicAnimation(), forKey: nil)
        backgroundCircleShapeLayer.add(Animation.type.opacityAnimation(), forKey: nil)
        backgroundCircleShapeLayer.add(Animation.type.pulseAnimation(), forKey: nil)
        downloaderTimer = Timer.scheduledTimer(timeInterval: 0.1, target: self, selector: #selector(updater), userInfo: nil, repeats: true)
        
    }
        @objc func updater() {
        foregroundCircleShapeLayer.setValue(Double(int_counter) / 100, forKey: "strokeEnd")
        downloadLabel.text = " \(int_counter) %"
        if int_counter == 100 {
            int_counter = 0
            foregroundCircleShapeLayer.removeAllAnimations()
            backgroundCircleShapeLayer.removeAllAnimations()
            downloadButton.isEnabled = true
            downloadLabel.layer.add(Animation.type.pulseAnimation(2), forKey: nil)
            downloaderTimer.invalidate()
            
        }
        int_counter += 1
        
    }
    
    
    override func viewDidLoad() {
        
        
        super.viewDidLoad()
        self.downloadButton.layer.cornerRadius = self.downloadButton.frame.height / 2
        self.downloadButton.clipsToBounds = true
        self.view.layer.addSublayer(backgroundCircleShapeLayer)
        self.backgroundCircleShapeLayer.position = view.center
        self.view.layer.addSublayer(foregroundCircleShapeLayer)
        self.foregroundCircleShapeLayer.position = view.center
        self.view.addSubview(downloadLabel)
        NSLayoutConstraint.activate([
            downloadLabel.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            downloadLabel.centerYAnchor.constraint(equalTo: view.centerYAnchor),
            downloadLabel.widthAnchor.constraint(equalToConstant: 150),
            downloadLabel.heightAnchor.constraint(equalToConstant: 150)
            ])
    }
    
    @IBAction func Twitter(_ sender: Any)
    {
        UIApplication.shared.open(URL(string: "#")! as URL, options : [:],completionHandler: nil)
    }
    
    @IBAction func Instagram(_ sender: Any)
    {
        UIApplication.shared.open(URL(string: "#")! as URL, options : [:],completionHandler: nil)
    }
}
