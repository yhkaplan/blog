---
title: "Code Generation in Swift 1"
date: 2018-07-07T12:18:28+09:00
draft: true
---

# Code Generation in Swift: Part 1

This entry is the first in a series that I plan on writing about code generation in Swift.

## Example

### Code

```swift
public struct LowerLabelSettings: Label, Equatable {
    public var text: String?
    public var layout: LowerLabelLayout
    public var textSettings: TextSettings

    public init(
        text: String? = nil,
        layout: LowerLabelLayout = LowerLabelLayout(),
        textSettings: TextSettings = TextSettings()
    ) {
        self.text = text
        self.layout = layout
        self.textSettings = textSettings
    }
}
```
### Generated Code

```swift
import Quick
import Nimble
@testable import MoonWalker

class LowerLabelLayoutTests: QuickSpec {

    override func spec() {

        describe("LowerLabelLayoutTests") {

            let leadingConstant = generateRandomCGFloat()
            let trailingConstant = generateRandomCGFloat()
            let bottomConstant = generateRandomCGFloat()
            let height = generateRandomCGFloat()

            let sut = LowerLabelLayout(
                leadingConstant: leadingConstant, trailingConstant: trailingConstant, bottomConstant: bottomConstant, height: height
            )

            it("leadingConstant is set") {
                let expected = leadingConstant
                let tested = sut.leadingConstant

                expect(tested).to(equal(expected))
            }

            it("trailingConstant is set") {
                let expected = trailingConstant
                let tested = sut.trailingConstant

                expect(tested).to(equal(expected))
            }

            it("bottomConstant is set") {
                let expected = bottomConstant
                let tested = sut.bottomConstant

                expect(tested).to(equal(expected))
            }

            it("height is set") {
                let expected = height
                let tested = sut.height

                expect(tested).to(equal(expected))
            }

        }
    }
}
```

### Template
https://github.com/yhkaplan/MoonWalker/blob/master/Templates/AutoTestable.stencil
```swift
{% macro declareProperties variables %}
    {% for variable in variables where variable.readAccess != "private" and variable.readAccess != "fileprivate" %}
         {% if variable.typeName|contains:"String" %}
            let {{variable.name}} = "test {{variable.name}}"
         {% elif variable.typeName|contains:"CGFloat" %}
            let {{variable.name}} = generateRandomCGFloat()
         {% elif variable.typeName|contains:"UIFont" %}
            let {{variable.name}} = UIFont.boldSystemFont(ofSize: 20.0)
         {% elif variable.typeName|contains:"UIColor" %}
            let {{variable.name}} = UIColor.green
         {% else %}
            {% if variable.typeName|contains:"UIView?" %}
            let {{variable.name}} = UIView()
            {% elif variable.typeName|contains:"UIImage?" %}
            let {{variable.name}} = UIImage()
            {% else %}
            let {{variable.name}} = {{variable.typeName}}()
            {% endif %}
         {% endif %}
    {% endfor %}
{% endmacro %}

{% macro declareTests variables %}
    {% for variable in variables where variable.readAccess != "private" and variable.readAccess != "fileprivate" %}
            it("{{variable.name}} is set") {
                let expected = {{variable.name}}
                let tested = sut.{{variable.name}}

                expect(tested).to(equal(expected))
            }

    {% endfor %}
{% endmacro %}

{% macro filloutInit variables %}
                {% map type.storedVariables into parameters using var %}{{ var.name }}: {{ var.name }}{% endmap %}
                {{ parameters|join:", " }}
{% endmacro %}

{% for type in types.types|!enum where type.implements.AutoTestable or type|annotated:"AutoTestable" %}
// sourcery:file:Generated/{{ type.name }}Tests
// MARK: - {{ type.name }}Tests

import Quick
import Nimble
@testable import MoonWalker

class {{type.name}}Tests: QuickSpec {

    override func spec() {

        describe("{{type.name}}Tests") {

            {% call declareProperties type.storedVariables %}

            let sut = {{type.name}}(
                {% call filloutInit type.storedVariables %}
            )

            {% call declareTests type.storedVariables %}
        }
    }
}
// sourcery:end
{% endfor %}
```
## Tools

## Starting Points

## Coming Up
