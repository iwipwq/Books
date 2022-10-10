# Part1 - Selections

리액트 타입스크립트 환경에서 D3 서브모듈을 설치합니다.

```
yarn add d3-selection @types/d3-selection
```

```tsx
import { useRef } from "react";

const D3TestPage: React.FC = () => {
    const svgRef = useRef<SVGSVGElement | null>(null);
    return (
        <div>
            <svg ref={svgRef} />
        </div>
    )
}

export default D3TestPage;
```
![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-07%20220804.png)

svg 타입은 `svgsvgElement` 입니다.

```tsx
import { useEffect, useRef } from 'react';
import { select } from 'd3-selection'

const D3TestPage: React.FC = () => {
    const svgRef = useRef<SVGSVGElement | null>(null);
    useEffect(() => {
      console.log(select(svgRef.current));
    }, [])
    

    return (
        <div>
            <svg ref={svgRef} />
        </div>
    )
}

export default D3TestPage;
```

`svgRef.current`는 해당 노드(혹은 요소)의 레퍼런스를 제공 해 줄 것이고,
`select`는 해당 요소(element)를 감싸주는 래퍼(wrapper)로 Dom element를 조정할 수 있는 프로퍼티와 메소드를 제공해줍니다.

한번 실행해봅시다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-07%20222347.png)

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-07%20222400.png)

프로퍼티에서 svg엘리먼트를 확인할 수 있습니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20125914.png)

디폴트 svg element는 300, 150 의 dimensions를 가지고 있습니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-07%20222801.png)

또한 selection 오브젝트와 사용할 수 있는 많은 메소드들을 제공해줍니다.

첫번째로 사용해볼 메소드는 `append` 입니다.

기본적으로 svg는 svg elements를 제공하고 있습니다.

예를 들면 이런 도형 같은 것들이 있습니다.

```tsx
            <svg ref={svgRef} >
                <line/>
                <rect/>
                <circle/>
            </svg>
```

여기에 `width` 같은 `attribute` 도 추가할 수 있습니다.

```tsx
            <svg ref={svgRef} >
                <line/>
                <rect width={100} height={100} fill="bule"/>
                <circle/>
            </svg>
```

이런것들을 이런식으로 하드코딩하지 않고 `d3`를 이용해 해볼겁니다.

바로 `append`를 이용하면 됩니다.

```jsx
import { useEffect, useRef } from 'react';
import { select } from 'd3-selection'

const D3TestPage: React.FC = () => {
    const svgRef = useRef<SVGSVGElement | null>(null);
    useEffect(() => {
      console.log(select(svgRef.current));
      select(svgRef.current)
        .append('rect')
    }, [])

    return (
        <div>
            <svg ref={svgRef} >
                <line/>
                <rect width={100} height={100} fill="bule"/>
                <circle/>
            </svg>
        </div>
    )
}

export default D3TestPage;
```

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-07%20223919.png)

그리고 select에서부터 메서드체이닝으로 도형을 만든뒤`.append('rect')` 도형의 attribute를 추가해줄 수 있습니다. `.attr('속성이름', 속성값)`

```jsx
    const svgRef = useRef<SVGSVGElement | null>(null);
    useEffect(() => {
      console.log(select(svgRef.current));
      select(svgRef.current)
        .append('rect')
        .attr('width', 100)
        .attr('height', 100)
        .attr('fill', 'blue')
    }, [])
```

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20045643.png)

그리고 위 코드는 JSX 엘리먼트를 이용해 만든 이 코드와 동일합니다.

```jsx
        <div>
            <svg ref={svgRef} >
                <rect width={100} height={100} fill="bule"/>
            </svg>
        </div>
```

그러면 이 JSX엘리먼트를 지우고 `<svg ref={svgRef} />` 만 남기고 실행시켜봅시다.

```jsx
import { useEffect, useRef } from 'react';
import { select } from 'd3-selection'

const D3TestPage: React.FC = () => {
    const svgRef = useRef<SVGSVGElement | null>(null);
    useEffect(() => {
      console.log(select(svgRef.current));
      select(svgRef.current)
        .append('rect')
        .attr('width', 100)
        .attr('height', 100)
        .attr('fill', 'blue')
    }, [])

    return (
        <div>
            <svg ref={svgRef} />
        </div>
    )
}
```

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20050648.png)

(App.tsx의 App컴포넌트에 text-align:center가 적용되어 있어서 가운데로 몰렸습니다.)

D3를 이용해 처음으로 svg를 만들어보았습니다.

이제 다른 selector를 사용해보겠습니다.
`selectAll` 입니다.

selectAll 역시 기본적으로 selection object를 제공해줍니다. 다만 하나의 엘리먼트 대신 여러개의 엘리먼트를 선택할 수 있습니다.

예를 들어 이렇게 `<rect/>` 가 여러개 있는 SVG container를 가지고 있다면,

```jsx
            <svg ref={svgRef}>
                <rect/>
                <rect/>
                <rect/>
            </svg>
```

이렇게 selectAll을 이용해 선택할 수 있습니다.

```jsx
      select(svgRef.current)
        .append('rect')
        .attr('width', 100)
        .attr('height', 100)
        .attr('fill', 'blue')
      selectAll('rect')
```

또 `w3c` 표준의 선택자(selector)를 select 할수도 있습니다.

예를들어 요소에 class 가 붙었을 경우에는,

```jsx
        <div>
            <svg ref={svgRef}>
                <rect className='foo'/>
                <rect/>
                <rect/>
            </svg>
        </div>
```
```jsx
selectAll('.foo')
```
클래스네임을 이용해 선택할 수 있습니다.

id도 마찬가지입니다.

```jsx
<rect id="foo" />
```
```jsx
selectAll('#foo');
```

이번시간에는 `rect` 로 지정해서 선택해보겠습니다.

마찬가지로 attributes를 추가해봅시다.

```jsx
import { useEffect, useRef } from 'react';
import { select, selectAll } from 'd3-selection'

const D3TestPage: React.FC = () => {
    const svgRef = useRef<SVGSVGElement | null>(null);
    useEffect(() => {
      console.log(select(svgRef.current));
    //   select(svgRef.current)
    //     .append('rect')
    //     .attr('width', 100)
    //     .attr('height', 100)
    //     .attr('fill', 'blue')
      selectAll('rect')
        .attr('width', 100)
        .attr('height', 100)
        .attr('fill', 'blue')
    }, [])

    return (
        <div>
            <svg ref={svgRef}>
                <rect className="foo"/>
                <rect/>
                <rect/>
            </svg>
        </div>
    )
}

export default D3TestPage;
```

그리고 실행해보겠습니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20054957.png)

지금은 겹쳐서 1개로 보이지만 개발자도구를 보면 3개의 엘레멘트가 존재하는것을 볼 수 있습니다.

이번엔 `x` attribute를 이용해 이 사각형들의 간격을 조절해보겠습니다.

그리고 x에 들어갈 속성값에는 상수대신 함수를 넣을겁니다.

```jsx
.attr('x', (_,index) => index * 100)
```

여기서 x에 들어가는 함수의 첫번째 인수는 `datum`으로 element가 들어가게 됩니다. 지금은 신경쓸필요가 없으니 `_`로 설정하겠습니다.

그리고 두번째 인수는 index를 나타내고 이것을 사용할 겁니다.

그리고 해당 인덱스 마다 100을 곱해준 값을 x에 넣어 거리를 벌려줄겁니다.

```jsx
import { useEffect, useRef } from 'react';
import { select, selectAll } from 'd3-selection'

const D3TestPage: React.FC = () => {
    const svgRef = useRef<SVGSVGElement | null>(null);
    useEffect(() => {
      console.log(select(svgRef.current));
    //   select(svgRef.current)
    //     .append('rect')
    //     .attr('width', 100)
    //     .attr('height', 100)
    //     .attr('fill', 'blue')
      selectAll('rect')
        .attr('width', 100)
        .attr('height', 100)
        .attr('fill', 'blue')
        .attr('x', (_,index) => index * 100)
    }, [])

    return (
        <div>
            <svg ref={svgRef}>
                <rect className="foo"/>
                <rect/>
                <rect/>
            </svg>
        </div>
    )
}

export default D3TestPage;
```

실행해봅시다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20055844.png)

좋습니다. 3개의 사각형이 100만큼씩 떨어져서 하나의 긴 사각형 처럼 보입니다.

다음시간에는 `data joins`에 대해 알아보고 돔 엘레멘트를 조정하기 위해 데이터를 어떻게 사용하는지에 대해 배워볼 겁니다.

# Part2 - Data Joins

이번시간에는 `Data Joins`에 대해 알아볼겁니다.

`Data Joins`은 `selection`에 데이터를 묶고 서로간의 관계를 형성하는 방법입니다.

이 데이터를 통해 SVG를 구현, 조작할 수 있습니다.

```tsx
import { select } from "d3-selection";
import { useEffect, useRef, useState } from "react";

const D3Part2:React.FC = () => {
    const ref = useRef(null);
    const [selection, setSelection] = useState(null);

    useEffect(() => {
        if(!selection) {
            setSelection(select(ref.current))
        }
    }, [selection]);

    return (
        <div>
            <svg ref={ref}/>
        </div>
    )
}

export default D3Part2;

```

먼저 useEffect로 selection 상태가 바뀔때마다 selection이 없다면 ref.current를 selection으로 설정하도록 만듭니다.

```tsx
setSelection(select(ref.current))
```
이 부분에서 타입에러가 날텐데요. `select`에 커서를 놓으면 타입을 알 수 있습니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20154914.png)

useState의 제네릭값을 이렇게 설정해줍니다.

```jsx
import { select, Selection } from "d3-selection";
import { useEffect, useRef, useState } from "react";

const D3Part2:React.FC = () => {
    const ref = useRef(null);
    const [selection, setSelection] = useState<null | Selection<null, unknown, null, undefined>>(null);

    useEffect(() => {
        if(!selection) {
            setSelection(select(ref.current))
        }

    }, [selection]);

    return (
        <div>
            <svg ref={ref}/>
        </div>
    )
}

export default D3Part2;
```

그 후 `Selection`타입을 `d3-selection`패키지에서 가져와줍니다.

참고로 useRef에 타입을 `<SVGSVGElement | null>`로 설정했을때 `Selection`타입은 이렇게 표시됩니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20155345.png)


```jsx
    useEffect(() => {
        if(!selection) {
            setSelection(select(ref.current))
        } else {
            selection
                .append('rect')
                .attr('height', 100)
                .attr('width', 200)
                .attr('fill', 'purple')
        }
    }, [selection]);
```

이어서 `selection`값이 존재하면 직사각형 요소를 그려줍니다.

하지만 이번에는 이 부분을 `.data` 메소드를 이용해 작성해보겠습니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20160348.png)

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20160327.png)

`data`는 `데이터배열을 반환하는 함수` 혹은 `데이터가 들어있는 배열`을 받습니다. 그리고 selection에 받아온 배열을 반환해줍니다.

이번시간에는 함수가 아닌 배열을 넣어볼겁니다.

모의 데이터를 컴포넌트 바깥에 하나 만들어놓습니다.
```jsx
const mockData = [{width: 200, height: 150, fill: 'orange'}];
```
그리고 data에 모의 데이터를 넣어줍니다.

```jsx
useEffect(() => {
    if(!selection) {
        setSelection(select(ref.current));
    } else {
        selection
            .data(mockData)
        console.log(selection)
    }
},[selection])
```

그리고 `selection`에 콘솔을 찍어 살펴보겠습니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20161938.png)

data메소드에 넣은 데이터가 selection의 돔 엘레멘트에 프로퍼티로 추가되었습니다.

그리고 data로 넣은 데이터는 이렇게 접근할 수 있습니다.

예를들어 attr로 width 값을 추가하고 싶다면 value에 함수를 넣고 첫번째 아규먼트에 접근하면 됩니다.

```jsx
    useEffect(() => {
        if(!selection) {
            setSelection(select(ref.current))
        } else {
            selection
                .data(mockData)
                .attr('width', _=>_.width)
            console.log(selection)
        }
    }, [selection]);
```

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20162303.png)

이렇게 하면 타입스크립트에서 자동완성 또한 제공해줍니다.

참고로 data를 넘겨주지 않는다면 첫번째 인수는 `undefined`가 됩니다.
```jsx
            .attr('x', (d,i) => {
                console.log("d",d);
                console.log("i",i);
                return i + 100;
            })
```
![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20163239.png)

이번에는 `<svg>` 안에 `<rect>` 여러개가 있다고 가정해봅시다.

```jsx
    return (
        <div>
            <svg ref={ref}>
                <rect/>
                <rect/>
                <rect/>
            </svg>
        </div>
    )
```

이런 경우에 모든 rect를 조작하려면 `selectAll`을 사용하면 되겠죠.

```jsx
selectAll('rect')
```

이번에는 데이터를 조금 변형해보겠습니다.
```jsx
const mockData2 = [
    {
        units: 150,
        color: 'purple' 
    },
    {
        units: 100,
        color: 'orange' 
    },
    {
        units: 50,
        color: 'pink' 
    },
]
```

그리고 데이터를 넘겨줍니다.
```jsx
            selectAll('rect')
                .data(mockData2)
```
그리고 데이터에 접근하기 위해 `attr`를 추가해줍니다.
```jsx
            selectAll('rect')
                .data(mockData2)
                .attr('width', 100)
                .attr('height', d => d.units)
                .attr('fill', d=> d.color)
                .attr('x', (_,i) => i * 100)
```
width값은 data에 존재하지 않으니 하드코딩해줍니다.

그리고 겹치지 않도록 간격을 벌려줍니다.

마지막으로 이런 질문에 대해 생각해봅시다.

현재 존재하는 `<rect/>` 수 보다 `data`에 들어있는 데이터량이 더 많을때 d3가 이것을 감지하고 `<rect/>`요소를 더 만드는 방법이 있을까요?

바로 `enter selection`을 이용하는 방법입니다.

`mockData2`에 데이터를 더 추가해보겠습니다.
```jsx
const mockData2 = [
    {
        units: 150,
        color: 'purple' 
    },
    {
        units: 100,
        color: 'orange' 
    },
    {
        units: 50,
        color: 'pink' 
    },
    {
        units: 70,
        color: 'bisque' 
    },
    {
        units: 120,
        color: 'aquamarine' 
    },
]
```

그리고 이번에는 전체를 `rects`에 할당한 뒤 이것을 콘솔로그로 살펴보겠습니다.

```jsx
    const rects = selection
        .selectAll('rect')
            .data(mockData2)
            .attr('width', 100)
            .attr('height', d => d.units)
            .attr('fill', d=> d.color)
            .attr('x', (_,i) => i * 100)
    console.log(selection)
    console.log(rects)
```
![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20171653.png)

그냥 `selection`과 달리 `rects`는  `_enter`,`_exit`,`_groups` 프로퍼티를 가지고 있습니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20171853.png)

그리고 `_enter` 프로퍼티를 열어서 살펴보면 0번째에 비어있는 배열 요소 3개와 EnterNode 2개 총 5개의 요소가 들어있음을 알 수 있습니다. `[빈 x3, EnterNode, EnterNode]` 

빈 배열 요소 3개는 렌더링된 3개의 사각형을 의미합니다. 그리고 mockData2의 각각의 데이터에 해당하는 나머지 두개의 가상 요소(virtual elements)는 아직 DOM에 입력(Entered)되지 않았습니다. 

그리고 `enter selection`을 통해 DOM에 아직 입력되지 않은 나머지 두 가상요소에 접근할 수 있습니다. `.enter()`

```jsx
    const rects = selection
        .selectAll('rect')
            .data(mockData2)
            .attr('width', 100)
            .attr('height', d => d.units)
            .attr('fill', d=> d.color)
            .attr('x', (_,i) => i * 100)

    rects.
        enter()
```

그리고나서 두 요소를 위한 rect를 똑같이 추가해보겠습니다.

```jsx
    const rects = selection
        .selectAll('rect')
            .data(mockData2)
            .attr('width', 100)
            .attr('height', d => d.units)
            .attr('fill', d=> d.color)
            .attr('x', (_,i) => i * 100)
    console.log(selection)
    console.log(rects)
    rects
        .enter()
        .append('rect')
        .attr('width', 100)
        .attr('height', d => d.units)
        .attr('fill', d=> d.color)
        .attr('x', (_,i) => i * 100)
```

그리고 기본 SVG 사이즈는 300 150 이기 때문에 svg Width를 500으로 설정하겠습니다.

```jsx
    return (
        <div>
            <h1>Part2 - Data Joins</h1>
            <svg ref={ref} width={500}>
                <rect/>
                <rect/>
                <rect/>
            </svg>
        </div>
    )
```

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20173453.png)

나머지 두 데이터 값에 해당하는 사각형이 렌더링되었습니다.

이렇게 `Data Joins`에 대해 알아보았습니다.

다음 시간에는 `Scales`에 대해 알아보겠습니다.

# Part3 - Scales

이번시간에는 `Scales`에 대해 알아보겠습니다.

```jsx
import { select, Selection } from "d3-selection";
import { useEffect, useRef, useState } from "react";

const D3Part3:React.FC = () => {
    const ref = useRef<SVGSVGElement | null>(null);
    const [selection, setSelection] = useState<null | Selection<SVGSVGElement | null, unknown, null, undefined>>(null);

    useEffect(() => {
        if(!selection) {
            setSelection(select(ref.current));
        }
    },[selection])
    return (
        <div>
            <h1>Part3 Scales</h1>
            <svg ref={ref} width={800} height={500}></svg>
        </div>
    )
}

export default D3Part3;
```

이번에는 큰 영역이 필요하기 때문에 svg 크기를 늘려줬습니다.

그리고 이번에 또다른 서브모듈을 설치할겁니다.

```ps1
yarn add d3-scale @types/d3-scale d3-array @types/d3-array
```

이제 `scale`에 대해 알아보겠습니다.

만약 데이터안의 숫자가 너무 크거나 너무 작아서 픽셀로 표현하기 곤란하다면 어떻게 해야할까요?

예를 들어 우리가 어떤 회사에 대한 재무자료를 가지고 있다면 아주 많은 숫자가 그 속에 들어있을겁니다. 

확실히 그 숫자들을 직접적으로 막대 그래프의 높이`height` 값에 전달하는 것은 말이 안될겁니다.

이런 문제를 해결할 수 있는 방안이 바로 `scale` 입니다.

먼저 모의데이터를 하나 작성합니다.

```jsx
const data = [
    {
        name: 'foo',
        number: 9000
    },
    {
        name: 'bar',
        number: 6456
    },
    {
        name: 'baz',
        number: 5264
    },
    {
        name: 'qux',
        number: 9199
    },
    {
        name: 'quux',
        number: 3164
    },
    {
        name: 'quuz',
        number: 2725
    },
]
```

그리고 `d3-scale`에서 `scaleLinear`를 불러옵니다.

```jsx
import { scaleLinear } from "d3-scale";
```
```jsx
const D3Part3:React.FC = () => {
    const ref = useRef<SVGSVGElement | null>(null);
    const [selection, setSelection] = useState<null | Selection<SVGSVGElement | null, unknown, null, undefined>>(null);

    const y = scaleLinear()
```

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20185648.png)

선형 축척(linear scale)은 주어진 범위(`domain`)에서 범위(`range`)로 숫자를 축척(스케일)하는 데 도움이 됩니다.

그리고 `도메인`과 `레인지`의 기본값은 `0`과 `1` 사이로 설정되어있습니다.

하지만 만약 우리가 도메인과 레인지 값을 특정하고 싶다면 어떻게 해야할까요?

```jsx
    const y = scaleLinear()
        .domain([])
        .range()
```

도메인은 아규먼트로 배열을 받습니다. 그리고 메서드 체이닝으로 레인지를 호출할 수 있습니다.

그리고 도메인 아규먼트에 들어가는 배열의 첫번째 요소가 바닥(floor)가 되고 두번째 요소가 천장(ceiling)이 됩니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20195912.png)

```jsx
    const y = scaleLinear()
        .domain([0, 10000])
        .range()
```
현재 data의 최대값은  9199이니, 도메인을 최소 0 최대 10000으로 설정해보겠습니다. 

```jsx
<svg ref={ref} width={800} height={500}></svg>
```
```jsx
    const y = scaleLinear()
        .domain([0, 10000])
        .range([0, 500])
```
그리고 현재 `<svg>`의 `height`값이 `500`이므로 레인지 값을 `0 ~ 500` 으로 설정하겠습니다.

```jsx
    const y = scaleLinear()
        .domain([0, 10000])
        .range([0, 500])

    useEffect(() => {
        if(!selection) {
            setSelection(select(ref.current));
        } else {
            console.log("y(0)", y(0));
            console.log("y(2315)", y(2315));
            console.log("y(8459)", y(8459));
        }
    },[selection])
```
![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20203711.png)

그리고 scaleLinear인 y에 값들을 집어넣어서 콘솔에 찍어보겠습니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20204122.png)

보시다시피 입력한 값들을 해당 범위 내에서 다운스케일 된 값으로 반환한 것을 보실 수 있습니다.

이 값을 막대그래프의 높이로 사용할 수 있을겁니다.

예를들어 우리가 `2315`의 값을 가지고 있다면 막대그래프 하나의 직사각형의 높이는 `115.75` 가 될 것입니다.

이제 직사각형을 그려봅시다.

```jsx
    useEffect(() => {
        if(!selection) {
            setSelection(select(ref.current));
        } else {
            console.log("y(0)", y(0));
            console.log("y(2315)", y(2315));
            console.log("y(8459)", y(8459));

            selection
                .selectAll('rect')
                .data(data)
                .enter()
                .append('rect')
                .attr('width', 100)
                .attr('x', (_,i) => i *100)
                .attr('fill', 'bisque')
                .attr('height', d => y(d.number))
        }
    },[selection])
```

먼저 `svg selection`에서 `rect`셀렉션을 모두 불러온뒤 `data`를 넘깁니다. 그리고 DOM에 입력되지 않은 가상요소들에 접근하기 위해 `enter()`로 접근한 뒤, 새 직사각형을 생성하고 높이값으로 `y()`를 통해 다운 스케일 계산된 값을 넣어줍니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20210611.png)

좋습니다. 여기에서 SVG 폭 전체에 걸쳐서 막대그래프 바를 확장시키고 싶다고 해봅시다.

이럴때 도움을 줄 수 있는 것이 `scaleBand` 입니다.
```jsx
import { scaleLinear, scaleBand } from "d3-scale";
```

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20211044.png)

```jsx
const x = scaleBand()
```

밴드 스케일은 x축을 담당할것이고 `range`를 균일한 대역(폭)(uniform band)으로 나누고 그걸 `domain`에 매핑할겁니다.

```jsx
    const x = scaleBand()
        .domain([])
```
`domain`은 우리가 가진 데이터를 대표하는 고유 식별자`unique identifier`를 가진 배열을 받습니다. 그리고 그래프에서 해당 식별자가 x 축에서 데이터가 무엇을 나타내는지 나타내기 때문에 알아보기 쉬워질 겁니다.

지금같은 경우엔 데이터의 `name` 값을 사용하면 되겠죠?

```jsx
    const x = scaleBand()
        .domain(data.map(d => d.name))
```
`data`를 `map`을 이용해 `name` 프로퍼티만 가진 배열을 새로 반환시킨 후 그것을 `domain`에 넘겨줍니다.

그리고 이 name을 가진 막대그래프 하나하나의 간격이 얼마가 될지 정해주어야합니다.

`range`로 x축 길이의 범위를 정할 수 있습니다. 
```jsx
<svg ref={ref} width={800} height={500}></svg>
```
```jsx
    const x = scaleBand()
        .domain(data.map(d => d.name))
        .range([0, 800]);
```
svg의 width가 800 이므로 `[0,800]` 값을 넘겨줍니다.

그리고 useEffect안의 x 간격을 벌려주는 식을 이런식으로 고칠겁니다.

```jsx
    useEffect(() => {
        if(!selection) {
            setSelection(select(ref.current));
        } else {
            console.log("y(0)", y(0));
            console.log("y(2315)", y(2315));
            console.log("y(8459)", y(8459));

            selection
                .selectAll('rect')
                .data(data)
                .enter()
                .append('rect')
                .attr('width', 100)
                .attr('x', d => x(d.name))
                .attr('fill', 'bisque')
                .attr('height', d => y(d.number))
        }
    },[selection])
```

그러면 타입스크립트가 에러를 뿜어낼텐데요

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20212451.png)

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20213339.png)

해당 값이 `undefined`를 반환 할 수도 있기 때문입니다.

`attr`이 원하는것은 값(`value`)과 `null`이지 `undefined`가 아니기 때문입니다.

따라서 다음과 같이 식을 변경할 수 있습니다.

```jsx
    .attr('x', d => {
        const valueX = x(d.name);
        if(valueX) {
            return valueX;
        }
        return null;
    })
```

`valueX`에 band scale된 `d.name` 값을 할당하고 `valueX` 가 존재하면 valueX값을 반환하고 없다면 `null`을 반환시킵니다.

혹은 에러가 발생하지 않을거라 확신한다면 단순히 `!`를 통해 해결할 수 도 있습니다.

```jsx
    .attr('x', d => x(d.name)!)
```

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20213439.png)

멋집니다. 일정간격으로 벌어진 그래프를 볼 수 있습니다. 

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20232020.png)

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20215435.png)

x의 위치는 각 직사각형의 끝인것을 확인할 수 있습니다.

band scale은 또한 width도 제공해줍니다.
바로 `bandWidth`라 불리는 프로퍼티입니다.

attr width값을 x.bandwidth로 고쳐보겠습니다.

```jsx
selection
    .selectAll('rect')
    .data(data)
    .enter()
    .append('rect')
    .attr('width', x.bandwidth)
    .attr('x', d => x(d.name)!)
    .attr('fill', 'bisque')
    .attr('height', d => y(d.number))
```
![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20232917.png)

bandwidth 값은 얼마일까요

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20233202.png)

width에 똑같이 적용된걸 확인할 수 있습니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20233213.png)

이번에는 그래프에 padding을 주고싶다고 생각해봅시다.

scaleband 메서드체이닝 중에 padding() 메서드를 이어붙일 수 있습니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20233450.png)

```jsx
    const x = scaleBand()
        .domain(data.map(d => d.name))
        .range([0, 800])
        .padding(0.8)
```

padding은 0과 1 사이로 이루어져있으며

1에 가까울 수록 패딩값이 커지고 0에 가까울 수록 패딩값이 줄어듭니다.

`0.8`을 주고 렌더링 해봅시다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20233727.png)

이번엔 `0.2` 값을 줘보겠습니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20233829.png)

그리고 만약 패딩을 바깥쪽이 아닌 안쪽에만 주고싶다면 이렇게 padding에 inner를 붙여서 `paddingInner`를 표기해 주면 됩니다.

```jsx
const x = scaleBand()
    .domain(data.map(d => d.name))
    .range([0, 800])
    .paddingInner(0.2);
```

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20234225.png)

명확한 비교를 위해 svg 영역을 명시하여 비교해보겠습니다.

- 그냥 패딩일 경우 `padding(0.2)`
![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20234329.png)

- 패딩 이너 `paddingInner(0.2)`
![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20234308.png)

반대로 `paddingOuter` 로 svg 영역 양쪽에서 패딩을 넣을 수 있습니다.

```jsx
    const x = scaleBand()
        .domain(data.map(d => d.name))
        .range([0, 800])
        .paddingInner(0.2)
        .paddingOuter(0.8)
```

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-08%20235147.png)

자, 지금 선형 스케일의 도메인값은 0 ~ 10000으로 고정되어 있습니다. 이제 만약에 선형 스케일의 도메인이 변화한다면 어떻게 될까요?

예를들어 데이터가 업데이트되고 제거되고 추가된다면 도메인을 하드코딩한다는 것은 좋지 못한 방법이 될겁니다.

그래서 우리는 `d3-array`의 `max` 함수를 사용할 수 있습니다.

```jsx
import { max } from 'd3-array'
```

`max`는 이터러블 가능한 요소(배열)나 함수를 전달받아 최대값을 구할 수 있습니다.

lodash에도 비슷한 함수가 있습니다.

```jsx
const maxValue = max(data, d => d.number)
```
첫번째 인수로는 비교할 데이터들이 들어있는 data 전체 배열을 넘겨줍니다.
그리고 두번째는 비교할 값을(혹은 값을 반환하는 함수) 넣어주면 됩니다.

그러면 `maxValue`는 `data`의 `number` 프로퍼티 중에서 가장 큰 값인 `9199` 값을 가지게 될겁니다.

그리고 이제 도메인의 최대값을 `maxValue`로 바꿔줍니다.

```jsx
const y = scaleLinear()
    .domain([0, maxValue])
    .range([0, 500])
```

그러면 또 이렇게 타입스크립트 에러가 발생합니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-09%20000547.png)

왜냐하면 max가 value 혹은 undefined를 반환하기 때문입니다.

전과 마찬가지로 조건문으로 값이 존재할 경우에만 반환하게 처리를 하거나 아니면 간단하게 `!`로 처리해도 됩니다.

```jsx
    const y = scaleLinear()
        .domain([0, maxValue!])
        .range([0, 500])
```
![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-09%20000721.png)

그리고 렌더링 해봅시다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-09%20001033.png)

domain의 최댓값이 계산되어 제일 큰 값인 네번째 9199 값에 height가 svg의 height와 동일하게 맞닿게 되었습니다.

이렇게 스케일에 대해 알아봤습니다.

다음 시간엔 `groups` 와 `margins`에 대해 알아보겠습니다.

# Part4 - Groups and Margins

이번시간에는 `group`에 대해 얘기해볼겁니다.

코드는 Part3에서 쓰던 걸 이어 쓰겠습니다.

SVG Group Element는 엘레멘트들을 그룹화시키는 데 도움을 주며 `<g>`로 정의할 수 있습니다.

몇몇개의 `rect`를 그룹화 시켜보겠습니다.

```jsx

    <svg ref={ref} width={800} height={500}>
        <g>
            <rect />
            <rect />
            <rect />
        </g>
    </svg>

```

`<g>`에는 그룹 자신에게 변형(transformation)을 가할 수 있고 변형사항은 안에 있는 직사각형들에게 반영됩니다. 

이는 막대차트 등에 있어서 도움이 되는 사항입니다.
왜냐하면 각각의 요소들에게 일일히 변형을 가하지 않고 모든 요소들을 한번에 적용 할 수 있기 때문입니다.

매우 중요한 점인데, 우리가 그래프에 축(axes)이나 라벨링(labeling)을 할때 간격이나 마진등을 조절해야할 때가 있습니다. 이때 막대그래프를 한꺼번에 조절이 가능합니다.

먼저 컴포넌트 함수 바깥에 영역(dimensions)을 오브젝트로 정의해보겠습니다.

```jsx
const dimensions = {
    width: 800,
    height: 500,
    chartWidth: 700,
    chartHeight: 400,
    marginLeft: 100,
}
```

그리고 해당 수치에 해당하는 수치들을 모두 이 변수를 참조하도록 바꿔주겠습니다. 추가적으로 헷갈리지 않도록 `paddingOuter`를 빼주겠습니다.

```jsx
const D3Part4:React.FC = () => {
    
    const ref = useRef<SVGSVGElement | null>(null);
    const [selection, setSelection] = useState<null | Selection<SVGSVGElement | null, unknown, null, undefined>>(null)

    const maxValue = max(data.map(d => d.number))

    const y = scaleLinear()
        .domain([0, maxValue!])
        .range([0, dimensions.chartHeight])

    const x = scaleBand()
        .domain(data.map(d=>d.name))
        .range([0, dimensions.chartWidth])
        .paddingInner(.5)
        // .paddingOuter(.8)

    useEffect(() => {
        if(!selection) {
            setSelection(select(ref.current))
        } else {
            selection
                .selectAll('rect')
                .data(data)
                .enter()
                .append('rect')
                .attr('fill','orange')
                .attr('x', d => {
                    const bandResult = x(d.name)!
                    console.log("band스케일된 x 값",d.name,bandResult)
                    console.log("bandwidth",x.bandwidth());
                    return bandResult
                })
                .attr('width', x.bandwidth )
                .attr('height', d=>y(d.number))
        }
    },[selection])

    return (
        <div>
            <h1>Part4 Groups and Margins</h1>
            <svg ref={ref} width={dimensions.width} height={dimensions.height}>
                <g>
                    <rect />
                    <rect />
                    <rect />
                </g>
            </svg>
        </div>
    )
}
```

이제 SVG영역을 가리키는 셀렉션과 해당 셀렉션에 SVG 영역만큼의 크기를 가지는 직사각형을 추가해보겠습니다.

```jsx
    useEffect(() => {
        if(!selection) {
            setSelection(select(ref.current))
        } else {
            selection
                .append('rect')
                .attr('width', dimensions.width)
                .attr('height', dimensions.height)
                .attr('fill', 'blue')
```

그리고 나중에 그룹에 변형을 적용시킬 수 있도록 `g`로 막대그래프인 `rect`를 감싸주고 그룹화시킬겁니다. 

```jsx
    selection
        .append('rect')
        .attr('width', dimensions.width)
        .attr('height', dimensions.height)
        .attr('fill', 'blue')

    selection
        .append('g')
        .selectAll('rect')
        .data(data)
        .enter()
        .append('rect')
        .attr('fill','orange')
        .attr('x', d => {
            const bandResult = x(d.name)!
            console.log("band스케일된 x 값",d.name,bandResult)
            console.log("bandwidth",x.bandwidth());
            return bandResult
        })
        .attr('width', x.bandwidth )
        .attr('height', d=>y(d.number))
```

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-09%20222936.png)

여기서 라벨링과 축을 추가하기 위해서는 그래프를 오른쪽으로 밀어야 할겁니다.

css의 `transform` 을 `attr`로 추가하면 됩니다.

```jsx
    selection
        .append('g')
        .attr('transform', `translate(${dimensions.marginLeft},0)`)
        .selectAll('rect')
        .data(data)
        .enter()
        .append('rect')
        .attr('fill','orange')
        .attr('x', d => {
            const bandResult = x(d.name)!
            console.log("band스케일된 x 값",d.name,bandResult)
            console.log("bandwidth",x.bandwidth());
            return bandResult
        })
        .attr('width', x.bandwidth )
        .attr('height', d=>y(d.number))
```

그리고 translate의 x축 값에 `dimensions.marginLeft`값을 넣어줍니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-09%20223629.png)

그럼 그래프가 `100px`만큼 오른쪽으로 이동한걸 확인할 수 있습니다.

이제 식별을 위해 추가한 파란 사각형을 지워주겠습니다.
```jsx
    // selection
    //     .append('rect')
    //     .attr('width', dimensions.width)
    //     .attr('height', dimensions.height)
    //     .attr('fill', 'blue')
```

그리고 이제 `axis`에 대해 말해볼건데요.

`d3-axis`는 `scales`를 인수로 받는 `axis generator` 함수를 지니고 있습니다.

먼저 `d3-axis`를 타입과 함께 설치합니다.

```ps1
yarn add d3-axis @types/d3-axis
```

그리고 두가지 함수를 불러올겁니다.

```jsx
import { axisLeft, axisBottom } from "d3-axis";
```

그리고 이 함수를 이용하는 방법은 간단하게 그냥 호출하고 가지고 있는 스케일을 전달 하면 됩니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-09%20224831.png)

x축도 마찬가지로 사용하면 됩니다.

```jsx
    const yAxis = axisLeft(y)
    const xAxis = axisRight(x)
```

그리고 만들어진 축을 렌더링 하기위해서  각각의 축에 대한 그룹을 생성해야합니다.

```jsx
    useEffect(() => {
        if(!selection) {
            setSelection(select(ref.current))
        } else {
            const xAxisGroup = selection
                .append('g')
```

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-09%20225510.png)

> ### Call()
>
>지정된 함수를 정확히 단 한번 호출하여, 이 셀렉션에 원하는(optional) 인수와 함께 전달합니다. 이 셀렉션을 반환합니다.
>
>@parma func - 선택한(optional) 인수와 함께 이 셀렉션을 첫번째 인수로 전달하는 함수입니다.
>
>@parma args - 콜백 함수에 전달될 인수의 목록입니다.



그리고 `.call`로 `xAxis`를 호출해줍니다.

```jsx
    const xAxisGroup = selection
        .append('g')
        .call(xAxis)

    const yAxisGroup = selection
        .append('g')
        .call(yAxis)
```

y 축도 마찬가지로 해주면 됩니다.

렌더링을 해봅시다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-09%20230811.png)

보시다시피 현재 렌더링은 svg를 기준으로 0,0 origin 지점에서부터 렌더링 되어있습니다.

여기서 이동시키려면 아까 전 막대그래프를 이동시킨 것처럼 `transform`을 `attr`로 추가하면 됩니다.

```jsx
const xAxisGroup = selection
    .append('g')
    .attr('transform', `translate(${dimensions.marginLeft},${dimensions.chartHeight})`)
    .call(xAxis)
```

오른쪽으로 100px 만큼 아래쪽으로 차트높이 만큼 이동시켰습니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-09%20231150.png)

잘 이동되었습니다. 이제 y축도 이동해봅시다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-09%20231432.png)

y축은 오른쪽으로만 100px 이동하면 되겠죠?

그리고 맨 위쪽의 0이 조금 짤렸습니다만 위쪽에 마진을 조금 주면 됩니다. 여기선 일단 넘어가겠습니다.

다음으로 각 축의 지표(틱)(tick)에 대해 얘기해보겠습니다.

만약 이 틱들의 범위가 마음에 들지 않았다고 해봅시다. 10개만 나오면 될것 같은데 너무 많이 나와서 3개로 압축시키고 싶습니다.

여기서는 `.ticks()`를 이용하면 됩니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-09%20231743.png)

```jsx
    const yAxis = axisLeft(y)
        .ticks(3)
```

yAxis 에서 메서드 체이닝으로 `ticks`를 연결하고 원하는 틱갯수를 인수로 넣어주면 됩니다. `.ticks(3)`

그런데 d3에선 최선을 다해서 3개의 틱들을 제공해주려 할겁니다만, 정확히 3개의 틱들을 언제나 줄 수 없기 때문에 아마 1~2개 정도의 차이가 날 수 도 있습니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-09%20232021.png)

이 같은 경우에는 5개로 렌더링 되었네요.

그리고 이번엔 틱의 형식(format)을 직접 바꾸고 싶다고 가정해봅시다.

`.tickFormat()` 함수를 이용하면됩니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-09%20232404.png)

`tickFormat()`은 인수나 틱자신을 나타내는 인수를 받고 틱을 반환하는 함수를 인수로 받습니다.

아무것도 입력하지 않았을때 현재 틱포멧자체를 반환합니다.

```js
    const yAxis = axisLeft(y)
        .ticks(3)
        .tickFormat(d => `$${d}`)
```

기존 틱데이터에 달러 표시를 붙여 출력해보겠습니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-09%20232751.png)

점점 더 그럴듯한 그래프에 가까워지고 있습니다.

여기서 y축의 방향과 막대그래프의 방향을 아래쪽에서 위로 향하도록 바꿔줘야겠죠, 이 방법을 다음시간에 알아볼 겁니다. 그리고 update에 대해 알아볼겁니다. 새 요소를 추가 수정 삭제하는 방법에 대한겁니다.

# Part5 - Update

이번시간에는 차트 축을 알맞게 이동시키는 방법과 차트에 요소를 추가 제거하는 방법에 대해 알아볼겁니다.

초기 세팅은 저번과 동일합니다.

```jsx
import { select, Selection } from "d3-selection";
import { useEffect, useRef, useState } from "react";

const initialState = [
    {
        name: 'foo',
        number: 90
    },
    {
        name: 'bar',
        number: 64
    },
    {
        name: 'baz',
        number: 52
    },
    {
        name: 'qux',
        number: 91
    },
    {
        name: 'quux',
        number: 31
    },
]

const D3Part5:React.FC = () => {
    const ref = useRef<SVGSVGElement | null>(null)
    const [selection, setSelection] = useState<null | Selection<SVGSVGElement | null, unknown, null, undefined>>(null)

    useEffect(() => {
        if(!selection) {
            setSelection(select(ref.current));
        }
    },[selection])

    return (
        <div>
            <h1>Part5 Update</h1>
            <svg ref={ref}></svg>
        </div>
    )
}

export default D3Part5;
```

이번에는 data 변수명을 initialState로 바꾼 뒤 useState를 통해 관리해보겠습니다.

```jsx
const [data, setDate] = useState(initialState)
```
이번에는 타입스크립트가 알아서 타입을 유추하기 때문에 따로 지정할 필요가 없습니다.

```jsx
const dimensions = {
    width: 900,
    height: 600,
}
```
svg 영역을 결정할 dimensions 객체도 컴포넌트 바깥에 만들어주겠습니다.

그리고 scaleBand linearSacle 을 d3-sclae로 부터 불러오고 max함수도 d3-array에서 불러옵니다.

```jsx
import { scaleBand, scaleLinear } from "d3-scale";
import { max } from "d3-array";
```

스케일을 생성합니다.

```jsx
    const y = scaleLinear()
        .domain([0, max(data.map(d => d.number))!])
        .range([0, dimensions.height])
```

우리 그래프의 최대값은 data.number 항목중 가장 큰 것이 될것이고 그 값을 0 ~ 600 사이로 다운스케일링 할것입니다.

```jsx
    const x = scaleBand()
        .domain(data.map(d=>d.name))
        .range([0, dimensions.width])
```

scaleBand도 지정해줍니다.
```jsx
<svg ref={ref} width={dimensions.width} height={dimensions.height}></svg>
```
svg 영역도 지정해줍니다.

```jsx
    useEffect(() => {
        if(!selection) {
            setSelection(select(ref.current));
        } else {
            selection
                .selectAll('rect')
                .data(data)
                .enter()
                .append('rect')
                .attr('width', x.bandwidth)
                .attr('height', d=>y(d.number))
                .attr('x', d=>x(d.name)!)
                .attr('fill', 'orange')

        }
    },[selection])
```

먼저 scaleLinear 의 range를 거꾸로 뒤집겠습니다.

```jsx
    const y = scaleLinear()
        .domain([0, max(data.map(d => d.number))!])
        .range([dimensions.height, 0])
```

그리고 새 attribute인 `y`에 대해 알아보겠습니다.
`x`와 동일하지만 y축을 담당하는 요소입니다.

그리고 이 요소에 스케일다운된 number를 넣어주겠습니다.

```jsx
            selection
                .selectAll('rect')
                .data(data)
                .enter()
                .append('rect')
                .attr('width', x.bandwidth)
                .attr('height', d=>y(d.number))
                .attr('x', d=>x(d.name)!)
                .attr('y', d=>y(d.number))
                .attr('fill', 'orange')
```

그리고 attr `height` 에는 다운스케일된 number 대신 `dimensions.height`를 넣어주겠습니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-10%20120527.png)

막대그래프의 높이는 600으로 고정되었지만 대신 y축을 이동해서 그래프의 높이를 맞추고 있습니다.

그러면 여기서 현재 높이인`dimension.height` 에서 y축을 이동한 만큼`d=>y(d.number)`을 빼주면 정확한 높이를 구할 수 있겠죠

```jsx
            selection
                .selectAll('rect')
                .data(data)
                .enter()
                .append('rect')
                .attr('width', x.bandwidth)
                .attr('height', d => dimensions.height - y(d.number))
                .attr('x', d=>x(d.name)!)
                .attr('y', d=>y(d.number))
                .attr('fill', 'orange')
```

구분을 위해 패딩을 좀 줘보겠습니다.
```jsx
    const x = scaleBand()
        .domain(data.map(d=>d.name))
        .range([0, dimensions.width])
        .paddingInner(0.1)
```

그리고 이제 `update` 에 대해 알아보겠습니다.

```jsx
    const addRandom = () => {

    }

    const removeLast = () => {
        
    }
```

첫번째로 랜덤요소를 추가하는 방법을 알아보겠습니다.

먼저 랜덤 문자열 생성기를 가져옵니다.

```ps1
yarn add randomstring @types/randomstring
```

```jsx
import randomstring from 'randomstring'
```

그리고 addRandom 안에서 랜덤스트링을 생성합니다.

기본 14글자를 생성해주는 여기서는 10을 넘겨주어 10글자로 생성합니다.

```jsx
    const addRandom = () => {
        const dataToBeAdded = {
            name: randomstring.generate(10),
            // number: Math.floor(Math.random()*(max-min) + min)
            number: Math.floor(Math.random()*(100-20) + 20)
        }

        setData([...data, dataToBeAdded])
        
    }
```

그리고 number도 설정해줍니다.

넘버는 `100` 과 `20` 사이의 랜덤한 숫자가 되도록 했습니다.

마지막으로 기존데이터에 `dataToBeAdded`를 추가해줍니다.

다음은 마지막 요소를 제거하는 방법입니다.

`slice`를 이용하는 방법인데요.

```jsx
    const removeLast = () => {
        if(data.length === 0) {
            return
        }
        const slicedData = data.slice(0, data.length-1)
        setData(slicedData);
    }
```
마지막 전 데이너까지만 잘라낸 배열을 상태변수에 넣는 방법입니다.

이제 `data`를 dependency로 갖는 새 `useEffect`를 만들건데요, 이것이 우리 데이터를 추적해줄 겁니다.
```jsx
    useEffect(() => {
        if(selection) {

        }
    }, [data]);
```
`scalelinear`과 `scaleBand`가 할당된 `x`,`y` 를 `const` 에서 `let`키워드로 바꿔줍니다.

```jsx
    let y = scaleLinear()
        .domain([0, max(data.map(d => d.number))!])
        .range([dimensions.height, 0])

    let x = scaleBand()
        .domain(data.map(d=>d.name))
        .range([0, dimensions.width])
        .paddingInner(0.1)
```

왜냐하면 데이터가 업데이트 되면 스케일의 도메인 최대값 수치 등등이 바뀔 수 있기 때문입니다.

```jsx
    useEffect(() => {
        if(selection) {
            y = scaleLinear()
                .domain([0, max(data, d=>(d.number))!])
                .range([dimensions.height, 0])

            x = scaleBand()
                .domain(data.map(d=>d.name))
                .range([0, dimensions.width])
                .paddingInner(0.1)

            const rects = selection.selectAll('rect').data(data)
        }
    }, [data]);
```

셀렉션이 존재한다면 스케일리니어와 스케일 밴드를 새 데이터로 정의하고 셀렉션에서 rect를 선택한 뒤 data를 join해줍니다.

그리고 이제 `exit`를 사용할 차례입니다.

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-10%20132957.png)

우리가 join한 데이터보다 더 많은 `DOM Element`를 가지고 있다면 `exit()`를 이용해 접근할 수 있습니다.(exit slection이 보관하고 있습니다.) `enter()`와 정반대의 상황이죠.

```jsx
            rects
                .exit()
```

그리고 남은 DOM Element 들은 필요없기 때문에 지워주면 됩니다.

```jsx
            rects
                .exit()
                .remove()
```

다음은 이미 DOM에 존재하는 직사각형(rect)들을 수용하거나 빼주기 위해 업데이트 해줄겁니다.

위쪽의 useEffect에서 attr코드들을 가져와 붙여넣어 줍니다.

```jsx
            rects
                .exit()
                .remove()

            rects
                .attr('width', x.bandwidth)
                .attr('height',d => dimensions.height - y(d.number))
                .attr('x', d=>x(d.name)!)
                .attr('y', d=>y(d.number))
                .attr('fill', 'orange')

```

그리고 업데이트 되는 데이터를 위한 enter() 셀렉션도 신경써줘야겠죠
enter 후 rect를 append 해주고 attr를 추가해줍니다.

```jsx
            rects
                .exit()
                .remove()

            rects
                .attr('width', x.bandwidth)
                .attr('height', dimensions.height - y(d.number))
                .attr('x', d=>x(d.name)!)
                .attr('y', d=>y(d.number))
                .attr('fill', 'orange')

            rects
                .enter()
                .append('rect')
                .attr('width', x.bandwidth)
                .attr('height', dimensions.height - y(d.number))
                .attr('x', d=>x(d.name)!)
                .attr('y', d=>y(d.number))
                .attr('fill', 'orange')
```

그리고 addRandom과 removeList를 호출할 수 있는 버튼을 만들어줍니다.

```jsx
    return (
        <div>
            <h1>Part5 Update</h1>
            <svg ref={ref} width={dimensions.width} height={dimensions.height}></svg>
            <button onClick={addRandom}>랜덤 데이터 추가하기</button>
            <button onClick={removeLast}>마지막 데이터 지우기</button>
        </div>
    )
```

렌더링해볼까요?

![](img/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-10%20193716.png)

데이터가 늘어나고 줄어드는 것을 볼 수 있습니다.

> 현재 randomstring 패키지가 react에서 작동하지 않는 에러가 있습니다. 여기서는 `name: randomstring.generate(10)` 대신 `name: '' + Math.floor(Math.random()*1e10)` 으로 숫자로 된 랜덤문자열을 생성했습니다.

# Part6 - Transitions

D3 Transition은 CSS transition과 비슷한 컨셉을 가지고 있습니다.

```jsx
const Button = styled.button`
    background-color: blue;
    transition-duration: 500ms;
    transition-timing-function: ease-in-out;

    &: hover {
        background-color: 'orange'
    }
`
```
point A 가 있고 point B 가 있죠 그리고 타이밍 함수와 듀레이션을 결정할 수 있습니다.

d3또한 이와 유사합니다. d3로 작성해보겠습니다.

먼저 d3-transition 모듈을 설치합니다.

```ps1
yarn add d3-transition @types/d3-transition
```

그리고 베이스 코드입니다.

```jsx
import styled from "@emotion/styled"
import { select, Selection } from "d3-selection"
import { useEffect, useRef, useState } from "react"
import 'd3-transition'

const Button = styled.button`
    background-color: 'blue';
    transition-duration: 500ms;
    transition-timing-function: ease-in-out;

    &:hover {
        background-color: 'orange';
    }
`

const D3Part6:React.FC = () => {
    const ref = useRef<SVGSVGElement | null>(null)
    const [selection, setSelection] = useState<null | Selection<SVGSVGElement | null, unknown, null, undefined >>(null)

    useEffect(() => {
        if(!selection) {
            setSelection(select(ref.current))
        } else {
            
        }
    }, [selection])

    return (
        <div>
            <h1>Part6 Transition</h1>
            <svg ref={ref} height={400}/>
        </div>
    )
}

export default D3Part6
```


```jsx
    useEffect(() => {
        if(!selection) {
            setSelection(select(ref.current))
        } else {
            selection
                .append('rect')
                .attr('width', 100)
                .attr('height', 100)
                .attr('fill', 'orange')
                
                .attr('fill', 'blue')
        }
    }, [selection])
```

여기에서 `fill` 속성을 `ornage`에서 `blue`로 바꾸려합니다.

```jsx
    useEffect(() => {
        if(!selection) {
            setSelection(select(ref.current))
        } else {
            selection
                .append('rect')
                .attr('width', 100)
                .attr('height', 100)
                .attr('fill', 'orange')
                .transition()
                .duration(2000)
                .attr('fill', 'blue')
        }
    }, [selection])

```

그렇다면 `.transition()`을 사용하면 됩니다.
`duration()` 도 `2000ms` 만큼 추가해줬습니다.

그러면 사각형의 색깔이 변화하는것을 볼 수 있습니다.

높이도 바꿔볼까요?

```jsx
            selection
                // ------ point A ------
                .append('rect')
                .attr('width', 100)
                .attr('height', 100)
                .attr('fill', 'orange')
                // - transition detail -
                .transition()
                .duration(2000)
                // ------ Point B ------
                .attr('fill', 'blue')
                .attr('height', 400)
```

사각형의 높이도 바뀌는것을 알 수 있습니다.

`ease` 또한 원하는 수치 혹은 프리셋으로 넣을 수 있습니다.

ease 프리셋을 사용하려면 `d3-ease` 패키지를 설치하면 됩니다

```ps1
yarn add d3-ease @types/d3-ease
```
```jsx
            selection
                .append('rect')
                .attr('width', 100)
                .attr('height', 100)
                .attr('fill', 'orange')
                .transition()
                .duration(2000)
                .ease(easeBounce)
                .attr('fill', 'blue')
                .attr('height', 400)
```
그리고 트랜지션 다음에 설정해주면 됩니다.

이제 Part5에서 작성했던 코드를 가지고 다시 시작해보겠습니다.

이번에는 Part5에서 만든 막대그래프를 아래쪽에서부터 위로 솟아오르도록 트랜지션을 넣고싶습니다.

그렇다면 먼저 시작점이 되는 `y`는 어떤 위치일까요? 맞습니다 바로 `svg` 영역의 `height`가 됩니다. 그리고 사각형`rect`의 `height`는 `0`이 되어야겠죠.

```jsx
            selection
                .selectAll('rect')
                .data(data)
                .enter()
                .append('rect')
                .attr('width', x.bandwidth)
                .attr('x', d=>x(d.name)!)
                .attr('fill', 'orange')
                // 시작지점
                .attr('height', d => dimensions.height - y(d.units))
                .attr('y', d=>y(d.units))
                
                // 트랜지션 내용

                // 끝나는 지점
                .attr('height', d => dimensions.height - y(d.units))
                .attr('y', d=>y(d.units))
```
먼저 이렇게 A지점과 B지점을 나눠놓겠습니다.

```jsx
    .attr('height', 0)
    .attr('y', dimensions.height)
    
    .attr('height', d => dimensions.height - y(d.units))
    .attr('y', d=>y(d.units))
```

그리고 앞서 말한대로 `height`는 `0`, `y`는 svg영역의 `height`로 설정합니다.

```jsx
    .attr('height', 0)
    .attr('y', dimensions.height)
    .transition()
    .duration(500)
    .attr('height', d => dimensions.height - y(d.units))
    .attr('y', d=>y(d.units))
```

그리고 트랜지션을 추가합니다.

[막대가 솟아나는 그림.gif]

자 그럼 이번엔 어떻게 막대가 순서대로 올라갈 수 있도록 만들 수 있을까요?

바로 `.delay()` 속성을 이용하면 됩니다.

`delay(1000)`이런식으로 하드코딩 해도 되지만, 이번에는 함수로 제어해보겠습니다.

```jsx
    .attr('height', 0)
    .attr('y', dimensions.height)
    .transition()
    .duration(500)
    .delay((_,i) => i * 100)
    .attr('height', d => dimensions.height - y(d.units))
    .attr('y', d=>y(d.units))
```

[순차적으로 시작되는 막대그래프 애니메이션.gif]

인덱스가 늘어남에 따라 딜레이를 증가시켜 순차적으로 애니메이션이 시작되도록 만들었습니다.

이번에는 ease를 넣어보도록 하죠

```jsx
    .attr('height', 0)
    .attr('y', dimensions.height)
    .transition()
    .duration(500)
    .ease(easeElastic)
    .delay((_,i) => i * 100)
    .attr('height', d => dimensions.height - y(d.units))
    .attr('y', d=>y(d.units))
```

[easeElastic이 적용된 탄성있는 애니메이션.gif]

`easeElastic`프리셋을 이용해 ease를 추가했습니다.

이번에는 업데이트 되는 막대그래프들에게 애니메이션을 부여해보겠습니다.

지금은 exit에 신경쓸필요 없이 .remove() 다음 rects 부터 수정하면 됩니다.

```jsx
    useEffect(() => {
        if(selection) {
            y = scaleLinear()
                .domain([0, max(data, d=>(d.units))!])
                .range([dimensions.height, 0])

            x = scaleBand()
                .domain(data.map(d=>d.name))
                .range([0, dimensions.width])
                .paddingInner(0.1)

            const rects = selection.selectAll('rect').data(data)

            rects
                .exit()
                .remove()

            rects
                // x 방향 트랜지션 추가위치
                .attr('width', x.bandwidth)
                .attr('height', d => dimensions.height - y(d.units))
                .attr('x', d=>x(d.name)!)
                .attr('y', d=>y(d.units))
                .attr('fill', 'orange')

            rects
                .enter()
                .append('rect')
                .attr('width', x.bandwidth)
                .attr('height', d => dimensions.height - y(d.units))
                .attr('x', d=>x(d.name)!)
                .attr('y', d=>y(d.units))
                .attr('fill', 'orange')
                // y 방향 트랜지션 추가 위치
        }
    }, [data]);
```

우리는 그저 이미 렌더링 된 rect에 대해서만 신경쓰면 됩니다.

그래서 트랜지션을 바로 주도록 할겁니다.

왜냐하면 포인트 A(시작포인트)는 이미 렌더링되어있고 그것이 시작 포인트이기 때문입니다.

```jsx
    rects
        // 트랜지션 추가위치
        .attr('width', x.bandwidth)
        .attr('height', d => dimensions.height - y(d.units))
        .attr('x', d=>x(d.name)!)
        .attr('y', d=>y(d.units))
        .attr('fill', 'orange')
```

[기존 막대들이 옆으로 밀려나오면서 새 막대가 나오는 막대그래프 애니메이션.gif]

기존 막대들은 옆으로 이동하면서 새 막대가 생성되는걸 볼 수 있습니다.

이제 아래쪽에서부터 솟아오르는 애니메이션만 추가하면 되겠네요.

```jsx
    rects
        .enter()
        .append('rect')
        .attr('width', x.bandwidth)
        .attr('x', d=>x(d.name)!)
        .attr('fill', 'orange')
        .attr('y', d=>y(d.units))
        .attr('height', d => dimensions.height - y(d.units))
        .transition()
        .duration(500)
        .ease(easeElastic)
        .attr('y', d=>y(d.units))
        .attr('height', d => dimensions.height - y(d.units))
```

[다 좋은데 기존 막대가 옆으로 밀려나는건 느리고 막대는 너무 빨리올라가서 겹쳐보여서 아쉬운.gif ]

막대가 밀려나는 애니메이션과 오르는 애니메이션이 겹쳐보이네요.
막대가 밀려나는 애니메이션이 실행된 뒤 막대가 솟아오르도록 밀려나는 애니메이션의 `duration`수치 만큼 `300` 딜레이를 추가해보도록 하겠습니다.

```jsx
    rects
        .enter()
        .append('rect')
        .attr('width', x.bandwidth)
        .attr('x', d=>x(d.name)!)
        .attr('fill', 'orange')
        .attr('y', dimensions.height)
        .attr('height', 0)
        .transition()
        .duration(500)
        .delay(300)
        .ease(easeElastic)
        .attr('y', d=>y(d.units))
        .attr('height', d => dimensions.height - y(d.units))
```

[멋지게 늘어나는 막대그래프.gif]

[그런데 지울때는 애니메이션이 없어서 그냥 없어지는 막대그래프.gif]

이제 `exit()` 부분으로 가서 마지막 데이터 제거 버튼을 눌렀을때 막대그래프가 위에서 아래로 사라지고 오른쪽에 있던 그래프가 옆으로 이동해 채워지는 애니메이션을 설정해보겠습니다.

```jsx
            rects
                .exit()
                // 트랜지션 추가위치
                .remove()
```

이미 `DOM`에 `rect`가 렌더링되어있기때문에 역시나 시작 포인트는 필요가 없습니다. 바로 트랜지션을 추가합니다.

```jsx
            rects
                .exit()
                .transition()
                .duration(300)
                .attr('y', dimensions.height)
                .attr('height', 0)
                .remove()
```

반대로 막대그래프가 없어지도록 `heigth` 를 `0`으로 `y`위치를 svg`height`로 설정했습니다.

[제거버튼을 누르자 위에서 아래로 사라지는 막대그래프, 그러나 옆으로 늘어나느 기존막대그래프 애니메이션과 겹침.gif]

이번에는 제거할때 애니메이션이 겹치지 않도록 제거가 끝난후 나머지 막대그래프가 늘어나도록 딜레이를 추가하겠습니다.

```jsx
    useEffect(() => {
        if(selection) {
            y = scaleLinear()
                .domain([0, max(data, d=>(d.units))!])
                .range([dimensions.height, 0])

            x = scaleBand()
                .domain(data.map(d=>d.name))
                .range([0, dimensions.width])
                .paddingInner(0.1)

            const rects = selection.selectAll('rect').data(data)

            rects
                .exit()
                .transition()
                .duration(300)
                .attr('y', dimensions.height)
                .attr('height', 0)
                .remove()

            rects
                .transition()
                .duration(300)
                .delay(100)
                .attr('width', x.bandwidth)
                .attr('height', d => dimensions.height - y(d.units))
                .attr('x', d=>x(d.name)!)
                .attr('y', d=>y(d.units))
                .attr('fill', 'orange')

            rects
                .enter()
                .append('rect')
                .attr('width', x.bandwidth)
                .attr('x', d=>x(d.name)!)
                .attr('fill', 'orange')
                .attr('y', dimensions.height)
                .attr('height', 0)
                .transition()
                .duration(500)
                .delay(250)
                .ease(easeElastic)
                .attr('y', d=>y(d.units))
                .attr('height', d => dimensions.height - y(d.units))
        }
    }, [data]);
```

조금 어색해보일 수 있으니 수치들을 조금씩 조정해줬습니다.

[멋지게 추가되고 삭제되는 그래프.gif]

훌륭합니다. 멋지게 해냈네요! 끝입니다. 나중에 봐요!
