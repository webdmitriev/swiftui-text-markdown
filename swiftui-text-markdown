//
//  ContentView.swift
//  mgubs-sui
//
//  Created by Oleg Dmitriev on 03.02.2025.
//

import SwiftUI

struct ContentView: View {
    let text = "*Oleg Dmitriev*, \n\n*Навыки:* \n~Swift~, ~UIKit~, ~SwiftUI~, ~SnapKit~, ~Core Data~  \n\n*Работа с сетью:* \n~REST API~, ~URLSession~, ~JSON~, \n\n*Дополнительно:* \n@red(Git), @007BFC(GitHub)"

    var body: some View {
        VStack {
            parseStyledText(text)
                .font(.body)
                .padding()
        }
    }

    // Основная функция для обработки текста
    func parseStyledText(_ input: String) -> Text {
        var result = Text("")
        let regex = try! NSRegularExpression(pattern: "(@[0-9A-Fa-f]{6}|@[a-zA-Z]+)\\((.*?)\\)|([*_~])(.*?)\\3", options: [])
        let matches = regex.matches(in: input, options: [], range: NSRange(input.startIndex..., in: input))
        
        var lastRangeEnd = input.startIndex
        
        for match in matches {
            let range = Range(match.range, in: input)!
            let prefix = String(input[lastRangeEnd..<range.lowerBound])
            result = result + Text(prefix)
            
            if let colorRange = Range(match.range(at: 1), in: input), let textRange = Range(match.range(at: 2), in: input) {
                let colorCode = String(input[colorRange]).dropFirst()
                let coloredText = String(input[textRange])
                let color = Color.fromString(String(colorCode))
                result = result + Text(coloredText).foregroundColor(color)
            } else if let styleCharRange = Range(match.range(at: 3), in: input), let textStyleRange = Range(match.range(at: 4), in: input) {
                let styleChar = String(input[styleCharRange])
                let styledText = String(input[textStyleRange])
                
                if styleChar == "*" {
                    result = result + Text(styledText).bold()
                } else if styleChar == "_" {
                    result = result + Text(styledText).italic()
                } else if styleChar == "~" {
                    result = result + Text(styledText).underline()
                }
            }
            
            lastRangeEnd = range.upperBound
        }
        
        result = result + Text(String(input[lastRangeEnd...]))
        return result
    }
}

extension Color {
    static func fromString(_ color: String) -> Color {
        let namedColors: [String: Color] = [
            "red": .red, "blue": .blue, "green": .green,
            "yellow": .yellow, "orange": .orange, "purple": .purple,
            "pink": .pink, "gray": .gray, "black": .black, "white": .white
        ]
        
        if let namedColor = namedColors[color.lowercased()] {
            return namedColor
        }
        
        if color.count == 6, let hexColor = Color(hex: color) {
            return hexColor
        }
        
        return .black
    }
    
    init?(hex: String) {
        let scanner = Scanner(string: hex)
        var hexNumber: UInt64 = 0
        if scanner.scanHexInt64(&hexNumber) {
            let r = Double((hexNumber & 0xFF0000) >> 16) / 255.0
            let g = Double((hexNumber & 0x00FF00) >> 8) / 255.0
            let b = Double(hexNumber & 0x0000FF) / 255.0
            self.init(red: r, green: g, blue: b)
        } else {
            return nil
        }
    }
}

#Preview {
    ContentView()
}
