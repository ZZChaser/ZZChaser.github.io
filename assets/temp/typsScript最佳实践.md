TypeScript 最佳实践
通用
1. 减少any, unknown, Record<string, string>等意义不明确的类型使用
2. 使用import type语法减少类型与变量的阅读混淆。
z通过使用import type和export type使得代码阅读更明确，避免一些编译器的[edge-case]，以及为相关工具或者IDE的类型分析提供更好的性能。
import type { Component } from "react";

interface ButtonProps {
    // ...
}
class Button extends Component<ButtonProps> {
    //               ~~~~~~~~~
    // error! 'Component' only refers to a type, but is being used as a value here.

    // ...
}

3. 对象定义顺序 readonly > 必选 > 可选 (结构清晰明了)
// bad
interface BaseInfo {
  age: number;
  nickname?: string;
  readonly name: string;
  weight?: number;
}
// good
interface BaseInfo {
  readonly name: string;
  age: number;
  nickname?: string;
  weight?: number;
}

4. 应提取共用的类型（避免冗余代码）
// bad
const man: { name: string; age: number } = { name: 'john', age: 30 };
const woman: { name: string; age: number } = { name: 'Anne', age: 32 };
// good
type Details = { name: string; age: number };
let man: Details = { name: 'john', age: 30 };
let woman: Details = { name: 'Anne', age: 32 };

5. 在 any 和 unknown 中选择 unknown（unknown 有更安全的类型检查）
// bad
let textData: any;
textData = '123';
textData = 123;
textData.trim(); // ts 检测通过
// good
let textData: unknown;
textData = '123';
textData = 123;
// textData.trim(); // ts 检测不通过
if (typeof textData === 'string') {
  textData.trim();
}

6. 定义数组类型使用 type[] 而不是 Array<type> （更简洁明了）
// bad
const numbers: Array<number> = [1, 2, 3];
// good
const numbers: number[] = [1, 2, 3];

7. 公用函数定义返回值类型，以防后续迭代维护出现问题
// bad
function getGoods() {
  return {
    name: 'test',
    price: 666,
  };
}
// good
interface Goods {
  name: string;
  price: number;
}
function getGoods(): Goods {
  return {
    name: 'test',
    price: 666,
  };
}

8. 建议 interface 命名不使用 I 前缀（https://github.com/inversify/InversifyJS/issues/242）
// bad
interface ICar {
  price: number;
}
// good
interface Car {
  price: number;
}

9. 类型定义：公共 api 使用 interface， Props 和 State 使用 type (公共 api 用 interface 便于声明合并合并。Props 和 State 用 type) https://medium.com/@martin_hotell/interface-vs-type-alias-in-typescript-2-7-2a8f1777af4c
10. 使用/** */注释来给类型做说明，鼠标悬浮时会有更好的提示（类型）
import React from "react";

interface MyProps {
  /** Description of prop "label".
   * @default foobar
   * */
  label?: string;
}

/**
 * General component description in JSDoc format. Markdown is *supported*.
 */
export default function MyComponent({ label = "foobar" }: MyProps) {
  return <div>Hello world {label}</div>;
}

11. 全局类型放在声明文件*.d.ts中，避免过多的 import 和 export
12. 联合类型比枚举更合适（https://fettblog.eu/tidy-typescript-avoid-enums/）
  1. 枚举编译后会侵入代码
 enum Direction {
    Up,
    Down,
    Left,
    Right,
}
// 编译后
"use strict";
var Direction;
(function (Direction) {
    Direction[Direction["Up"] = 0] = "Up";
    Direction[Direction["Down"] = 1] = "Down";
    Direction[Direction["Left"] = 2] = "Left";
    Direction[Direction["Right"] = 3] = "Right";
})(Direction || (Direction = {}));

  2. 数字枚举类型不安全
enum Direction {
  Up,
  Down,
  Left,
  Right,
}

declare function move(direction: Direction): void;

move(30); // 编译通过

  3. 字符枚举不属于结构性类型
enum Status {
  Admin = "Admin",
  User = "User",
  Moderator = "Moderator",
}

declare function closeThread(threadId: number, status: Status): void;

closeThread(10, "Admin"); // 编译不通过

  4. 联合类型最大的问题导致魔法字符串，后续修改可能造成困扰，可使用 const Object 解决
const Direction = {
  Up: 0,
  Down: 1,
  Left: 2,
  Right: 3,
} as const;

// Get to the const values of any object
type Values<T> = T[keyof T];
// Values<typeof Direction> yields 0 | 1 | 2 | 3
declare function move(
  direction: Values<typeof Direction>): void;

move(Direction.Down); // 编译通过
move(1); // 编译通过
move(30); // 编译不通过

组件
- props 和 deafaultProps
13. 使用普通函数注释方法注释组件，不使用 React.FC; 如果 Prop 类型多需要抽离成单独的 type.
// Declaring type of props - see "Typing Component Props" for more examples
type AppProps = {
  message: string;
}; /* use `interface` if exporting so that consumers can extend */

// Easiest way to declare a Function Component; return type is inferred.
const App = ({ message }: AppProps) => <div>{message}</div>;

// you can choose annotate the return type so an error is raised if you accidentally return some other type
const App = ({ message }: AppProps): JSX.Element => <div>{message}</div>;

// you can also inline the type declaration; eliminates naming the prop types, but looks repetitive
const App = ({ message }: { message: string }) => <div>{message}</div>;

14. 函数组件使用 ES6 默认值而不是 defaultProps（https://medium.com/@matanbobi/react-defaultprops-is-dying-whos-the-contender-443c19d9e7f1）
// bad
const Test = (props: TestProps) => {
  // ...
};
Test.defaultProps = {
  name: 'test',
};
// good
const Test = ({ name = 'test' }: TestProps) => {
  // ...
};

15. 公共组件的类型需要完善，并且带有适当的注释
import React from "react";

interface MyProps {
  /** Description of prop "label".
   * @default foobar
   * */
  label?: string;
}

/**
 * General component description in JSDoc format. Markdown is *supported*.
 */
export default function MyComponent({ label = "foobar" }: MyProps) {
  return <div>Hello world {label}</div>;
}

16. 扩展原生组件
import React from "react";

type Props = React.ComponentPropsWithoutRef<"button">;

function Button({ children, onClick, type }: Props) {
  return (
    <button onClick={onClick} type={type}>
      {children}
    </button>
  );
}

17. 组件类型就近声明
hooks
- hooks 返回两个值使用 const 常数类型转换为元祖，超过两个，用对象而不是元祖
// bad
function useTest() {
  const [data, setData] = useState(0);
  // ...
  return [data, setData];
}
// good
function useTest() {
  const [data, setData] = useState(0);
  // ...
  return [data, setData] as const;
}

// bad
function useTest() {
  const [data, setData] = useState(0);
  const [loading, setLoading] = useState(false);
  // ...
  return [data, setData, loading] as const;
}
// good
function useTest() {
  const [data, setData] = useState(0);
  const [loading, setLoading] = useState(false);
  // ...
  return { data, setData, loading };
}

- 使用useForm时注入表单的返回值类型
const [chargeForm] = Form.useForm<ChargeFormProps>();
// typeof values === ChargeFormProps
const values = await chargeForm.validateFields();

- useRef
  /**
    * @version 16.8.0
  */
  function useRef<T>(initialValue: T): MutableRefObject<T>;
  function useRef<T>(initialValue: T|null): RefObject<T>;
  function useRef<T = undefined>(): MutableRefObject<T | undefined>;

组件或者元素的的引用使用RefObject类型

  // bad
  const TaskList = () => {
    const actionRef = React.useRef<TableQueryActions>();
    return (
      <ListContent
        ref={actionRef} // 检测报错 Type 'MutableRefObject<TableQueryActions<any> | undefined>' is not assignable to type 'Ref<TableQueryActions<any>> | undefined'.
        // ...
      />
    );
  };
  
  // good
  const TaskList = () => {
    const actionRef = React.useRef<TableQueryActions>(null);
    return (
      <ListContent
        ref={actionRef}
        // ...
      />
    );
  };


- useContext
  - 推断类型 - 有明确初始值可以不用再单独提取类型
const AppContext = createContext({
  authenticated: true,
  lang: 'en',
  theme: 'dark',
});

const MyComponent = () => {
  const appContext = useContext(AppContext); //inferred as an object
  return <h1>The current app language is {appContext.lang}</h1>;
};

  - 给定类型 - 没有明确初始值单独提取类型
type Theme = 'light' | 'dark';
const ThemeContext = createContext<Theme>('dark');
const App = () => {
  const theme = useContext(ThemeContext);
  return <div>The theme is {theme}</div>;
};

三方组件库
- 如果基于第三方组件进行二次开发，尽量使用继承或者联合类型第三方组件自己声明的类型，而不是选择重写或者覆盖
// 代码来源 Freud/http
export interface IHttpConfig extends AxiosRequestConfig, IHttpCustomConfig {
  [configKey: string]: unknown;
}
export default function apply(instance: AxiosInstance): void {
  instance.interceptors.request.use(function requestAuthHandler(config) {
 const { provideAuth } = config as IHttpConfig;
 return provideAuth ? provideAuth(config as IHttpConfig) : config;
  });
}
// 如果config的类型直接设置为IHttpConfig，会报错，不符use的参数
// (value: AxiosRequestConfig) => AxiosRequestConfig | Promise<AxiosRequestConfig>

- 如果第三方组件库没有导出类型
import { Button } from 'library'; // but doesn't export ButtonProps! oh no!
type ButtonProps = React.ComponentProps<typeof Button>; // no problem! grab your own!
type AlertButtonProps = Omit<ButtonProps, 'onClick'>; // modify
const AlertButton: React.FC<AlertButtonProps> = (props) => (
  <Button onClick={() => alert('hello')} {...props} />
);

Troubleshooting
- 没有类型的第三方库
查找DefinitelyTyped /TypeSearch下面是否有声明
自己在src/types声明
declare module 'History' {
  interface Location {
    query: { [key: string]: string | number };
  }
}

- 全局的变量的库
declare global {
  interface Window {
    APP_METADATA: Record<string, unknown>;
  }
}

- 官方的 Bug type
// tsconfig
{
  "compilerOptions": {
    "paths": {
      "mobx-react": ["../typings/modules/mobx-react"]
    }
  }
}

// my-typings.ts
declare module "plotly.js" {
  interface PlotlyHTMLElement {
    removeAllListeners(): void;
  }
}

declare module "plotly.js"; // each of its imports are `any`

// MyComponent.tsx
import { PlotlyHTMLElement } from "plotly.js";

const f = (e: PlotlyHTMLElement) => {
  e.removeAllListeners();
};

- tsconfig.json / typescript-eslint 配置

建议开启 strict: true 的模式，并开启 noImplictAny；
 更多 tsconfig 模板：https://github.com/tsconfig/bases
{
  "compilerOptions": {
    "incremental": true,
    "outDir": "build/lib",
    "target": "es5",
    "module": "esnext",
    "lib": ["DOM", "ESNext"],
    "sourceMap": true,
    "importHelpers": true,
    "declaration": true,
    "rootDir": "src",
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "allowJs": false,
    "jsx": "react",
    "moduleResolution": "node",
    "baseUrl": "src",
    "forceConsistentCasingInFileNames": true,
    "esModuleInterop": true,
    "suppressImplicitAnyIndexErrors": true,
    "allowSyntheticDefaultImports": true,
    "experimentalDecorators": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "build", "scripts"]
}

fiture-spec
- 为常量声明as const属性
export const AuthCode = {
  ADD: 'add',
} as const;

// no error
AuthCode.ADD = 'www';

// 无法分配到 "ADD" ，因为它是只读属性。
AuthCode.ADD = 'www';

- 声明组件的Authconfig属性
namespace React {
  interface FunctionComponent {
    authConfig?: AuthConfig;
  }
}

- service 的函数需要类型注解表明返回的值得类型
    1. 在应用模块与service之间增加一层（比如叫moduleService），模块内请求的服务都在moduleService中定义，并注解好入参类型与返回类型，数据模型的转换也可以在这一层做。
// moduleService
import api from '@/services';
import { IHttpConfig } from '@freud/http/lib/interface';

type GetListParams = {
  id: number;
};

type List = { id: number; name: string }[];

const transform = (list: List) => {
  // ...
  return list;
};

export async function getList(params: GetListParams, options?: Partial<IHttpConfig>): Promise<List> {
  const list = await api.getItemPage(params, options);
  return transform(list);
}

// 模块内使用

import { getList } from './moduleService';

const init = async () => {
  const list = await getList({ id: 8 });
  //   ...
};

  1. 许多函数没有类型注释
  2. 注意使用as const来代替string类型
const tableProps = {
  fields: tableFields,
  rowKey: 'id',
  size: 'small' as const,
  total: productList?.total,
  pagination: {
    defaultPageSize: 5,
    pageSizeOptions: ['5', '10'],
  },
  data: productList?.data,
  rowSelection: {
    type: 'radio' as const,
    ...rowSelection,
  },
};

- tableFields的类型严格声明其类型，需要避免类型推断；否则依赖类型推断的值可能会有额外的值加入其中。
const listTableFields: TableFields = {
    key: 'name',
    required: true,
    name: '标题',
};

- 环境变量
// ambient variable
declare let process: {
  env: {
    NODE_ENV: 'development' | 'production';
  };
};

