# �Ż�

> ������һ���������õ�ʱ�⡣���Ѿ������һ��Ĺ���������������վ�����������ˡ�
> 
> -- Kazuo Ishiguro,*The Remains of the Day*

����һ�ס���°¶������һ����һ�³�Ϊ*lagniappe*��������͸��˿͵�һЩ���ⶫ�������Ѿ������������һ�������������������ϣ����� clox ����һЩ����Ȥ�ĺڿ͹�������һ�Σ�����Ҫ׷�󴿴�����ܡ����ǽ��������Ӧ�����ֽ�Ȼ��ͬ���Ż����ڴ˹����У������������ܵ������͸Ľ�����ʵ�ֻ��κγ�������ܡ�

## 30.1 ��������

**�Ż�**��ζ��ʹӦ�ó���������������ܡ�һ���Ż��ĳ�����ͬ�������飬ֻ����Ҫ���ٵ���Դ���������Ż�ʱͨ���뵽����Դ������ʱ�ٶȣ��������ڴ�ʹ�á�����ʱ�䡢�־ô洢��С���������Ҳ����Ҫ������������Դ����һЩ�ɱ�������ʹ�ɱ���Ҫ���˷�����ʱ�䡪������Ż�����ͨ�����лر���

�ڼ�������ڣ���һ��ʱ�䣬�����ĳ���Ա�������Ժ�����������Ӳ���ܹ��ͱ������ܵ�����ͨ������˼���������������ܡ���Щ��������һȥ�������ˣ���΢���롢�����С���֧Ԥ�⡢��ȱ������ܵ����Ӵ��ָ������ڷֿ��ˡ�����ϲ����װ C ��һ�֡��ͼ������ԣ������� ��

```js
printf("Hello, world!");
```

����Ļ�ϳ��ֵ��ʺ���  ֮��ļ���ջ �ߵ����ˡ�

������Ż���һ�ž����ѧ�����ǵĳ�����һֻ�߾�����Ȯ���Ӳ�����ϰ�ѵ�����������������������ص����յ㣬���ǾͲ���������˼��Ȯ������ѧ��ֱ�������Ϯ���෴��������Ҫ�۲����ı��֣������������������Ȼ��Ϊ���ҵ������·����

��������ѵ��ֻ���һֻ����һ���ϰ���һ�������ǲ��ܼ���������Ż���ʹ ����Lox ���� ������Ӳ��  �����еø��졣��ͬ�� Lox ����ǿ�� VM �Ĳ�ͬ���򣬲�ͬ�ļܹ�������ȱ�㡣

### 30.1.1  ��׼

�����������¹���ʱ������ͨ����д��������֤��ȷ�ԡ���ʹ���¹��ܲ���֤ VM ��Ϊ�� Lox ���򡣲���ȷ�����岢ȷ�������������¹���ʱ�����ƻ����й��ܡ������ܷ��棬���������Ƶ�����

1. ���������֤�Ż�*ȷʵ*��������ܣ�����˶��٣�

2. �������ȷ����������صĸ��Ĳ���*����*���ܣ�

����Ϊʵ����ЩĿ�����д�� Lox ������**��׼ (benchmarks)**����Щ�Ǿ�����Ƶĳ���ǿ������ʵ�ֵ�ĳЩ���֡����Ǻ����Ĳ��ǳ�����*��ʲô*������ִ������Ҫ�೤ʱ�䡣

> �������׼��������ʱ�䡣���ǣ���Ȼ�������ջᷢ���Լ���Ҫ��д��׼�����������ڴ���䡢�����ռ������ѵ�ʱ�䡢����ʱ��ȡ�

ͨ����������ǰ���׼�����ܣ��������˽���ĵ�Ч�������������Ż�ʱ�����в��Ե���Ϊ��Ӧ����֮ǰ��ȫ��ͬ����ϣ����׼�������еø��졣

һ��������һ������*��*��׼���ԣ��㲻�����Ժ���*�Ż�*�ı����ܣ� �����Ժ��������*����*��ͨ�����ᷢ��һЩ��׼���Ա�ø��죬����һЩ���ø�����Ȼ�����������������ʵ��������ִ�������Ż��������ѵľ�����

��ѡ���д�Ļ�׼�׼��Ǹþ����Ĺؼ����֡��������Ĳ���Χ����ȷ��Ϊ�����ӱ�������ѡ��һ�������Ļ�׼�������ܷ������ȿ��ǵ����֡����ǽ�ָ����ʵʩ��Щ�Ż�����������ѡ�����Ļ�׼�����Ҳ�Ҫ���Ƕ��ڷ�˼�����Ƿ����ڰ�����ʵ�ָ����Ŀ�ꡣ

> �� JavaScript VM ��������ɢ�У���һ���㷺ʹ�õĻ�׼�����׼������� WebKit �� SunSpider�����������ս�ڼ䣬Ӫ����Աʹ�� SunSpider ����������ǵ�����������ġ���߶ȼ����� VM �ڿ������Щ��׼�����Ż���
> 
> ���ҵ��ǣ�SunSpider ����ͨ������ʵ����� JavaScript ��ƥ�䡣���Ǵ����΢��׼���ԡ���������ɵ�С��߳�����Щ��׼���Ի�ͷ����ӵļ�ʱ����������Щ�����������ٶȽ�������һ�� JIT ���㹻��ʱ�����Ż������±����ȴ���·�����ͻ��ø��졣���� VM �ڿ�������һ�����ҵľ��أ����Ǳ������� SunSpider ������ø��ã�����ʵ���Ż���ʵ�û����еĳ�������֮������ѡ��
> 
> �ȸ�� V8 �Ŷ�ͨ���������ǵ� Octane ��׼�׼���Ϊ��Ӧ�����׼��ڵ�ʱ���ӽ���ʵ����Ĵ��롣��������� JavaScript ʹ��ģʽ�Ĳ��Ϸ�չ������ Octane Ҳʧȥ�������ô����������Ļ�׼���Խ���������������̬ϵͳ�ķ�չ����չ��
> 
> ���ס������Ŀ������*�û�����*���죬����׼����ֻ��һ��������

��׼������һ��΢��������������һ��������Ҫ�ڲ������������ʵ�ֵ�ͬʱȷ����׼ȷʵ���������ĵĴ���·����������������ʱ������Ҫ������ CPU �����������������ֵ�Ӳ���Ͳ���ϵͳ�������Ĳ��졣�Ҳ������������һ�������Ĳ��������ǽ���׼������Ϊһ������ʵ������ߵ��������ܡ�

### 30.1.2 ���� Profiling

�õģ��������Ѿ�����һЩ��׼�������������ߵø��졣��ô�죿���ȣ��������Ѿ�����������Զ��׼��ļ򵥹�����������ʹ����ȷ���㷨�����ݽṹ�������ߣ����٣���û��ʹ�����ش�����㷨�����ݽṹ���Ҳ���Ϊʹ�ù�ϣ��������������ͨ���޴��δ�������顰�Ż�������˵�ǡ����õ��������̡���

����Ӳ��̫���ӣ��޷����ݻ���ԭ���ƶ����ǳ�������ܣ�������Ǳ���ʵ�ؿ��졣����ζ��*����*��**̽���� (profiler)**���������δʹ�ù��Ļ�����һ�����г����ڴ���ִ��ʱ����Ӳ����Դʹ������Ĺ��ߡ��򵥵���ʾ���ڳ����е�ÿ�������ϻ����˶���ʱ�䡣���ӵļ�¼���ݻ���δ���С�ָ���δ���С���֧Ԥ������ڴ����͸�������ָ�ꡣ

> ����ġ���ĳ�����ָ����*����*Lox ����� Lox VM �������������ڳ����Ż� clox���������û��� Lox �ű�����Ȼ��ѡ���ĸ� Lox ������ص����ǵ� VM �н������Ӱ�� clox ����Щ�����ܵ�ѹ��������ǻ�׼���������Ҫ��ԭ��
> 
> ̽����*����*��������ʾ�������еĽű��е�ÿ��*Lox*���������˶���ʱ�䡣���Ǳ����д�Լ��ġ�Lox ̽����������������һ�㣬����΢�����˱���ķ�Χ��

��������Ը��ֲ���ϵͳ�����Եķ���������������ʲôƽ̨�ϱ�̣���ֵ����Ϥһ�������ķ��������㲻��Ҫ��Ϊ��ʦ�����ڽ������Ӹ������������Ӻ��ѧ����һЩ����������Щ��������Ҫ*����*����ͨ�����������Լ����֡������������������Ĺ��ߡ�

## 30.2 ���ٹ�ϣ��̽��

˵�ù����ˣ������ǿ�һЩ���Ϻ����ҵ�����ͼ������ʵ֤��������Ҫ���ĵ�һ���Ż��ǿ��Զ� VM ���е�*��С���ܸ��ġ�*

���ҵ�һ�εõ� clox �������ֽ��������ʱ���������κ�������� VM �ڿͶ����������顣��ƴ���˼�����׼��������һ������������ͨ���ҵĽ�����������Щ�ű������� Lox �����Ķ�̬���������У��ܴ�һ�����û��������ֶη��ʺͷ������ã������ҵ�һ����׼���Կ�������������

```js
class Zoo {
  init() {
    this.aardvark = 1;
    this.baboon   = 1;
    this.cat      = 1;
    this.donkey   = 1;
    this.elephant = 1;
    this.fox      = 1;
  }
  ant()    { return this.aardvark; }
  banana() { return this.baboon; }
  tuna()   { return this.cat; }
  hay()    { return this.donkey; }
  grass()  { return this.elephant; }
  mouse()  { return this.fox; }
}

var zoo = Zoo();
var sum = 0;
var start = clock();
while (sum < 100000000) {
  sum = sum + zoo.ant()
            + zoo.banana()
            + zoo.tuna()
            + zoo.hay()
            + zoo.grass()
            + zoo.mouse();
}

print clock() - start;
print sum;
```

> �����׼����Ҫ����������һ������*ʹ��*��ִ�еĴ���Ľ����ͨ����������ܺͲ���ӡ���������ȷ�� VM*����*ִ������ Lox ���롣����һ����Ҫ��ϰ�ߡ������Ǽ򵥵� Lox VM ��ͬ���������������л����������������������㹻���������Զ���������δʹ�ù��ļ��㡣
> 
> ���������ԺڿͶ� VM ��ĳЩ��׼�����еĳ�ɫ����ӡ����̣�ֻ����ʶ��������Ϊ��������������׼���Գ����Ż���һ���Ǵ���

�������ǰ��δ������׼���ԣ�����ܿ������ܿ�Ц��*��*����ô���£��ó����������������κ����õ����顣���������ǵ���һ�ѷ���������һ���ֶΣ���Ϊ��Щ�����Ǹ���Ȥ�����Բ��֡��ֶκͷ��������ڹ�ϣ���У����������Ҫע�����*һЩ*��Ȥ�ļ�����Щ���С���һ�ж�������һ����ѭ���У���ȷ�����ǵ�̽�������㹻��ִ��ʱ���������о����鿴ѭ����ȥ��

> ����������Թ�ϣ�����ܽ��л�׼���ԣ���Ӧ��ʹ�����಻ͬ��С�ı��������ڴ˴����ӵ�ÿ���������������������Գ�����ϣ���İ�Ԫ����С��ֵ�����Ҳ��������׳�һ���޴�Ļ�׼���Խű��������Ը�⣬�����������Ӹ����С�������ʳ��

���Ҹ�����̽��������չʾ��ʲô֮ǰ���뻨һ����ʱ�����һЩ�²⡣����Ϊ VM ���Ѵ󲿷�ʱ���� clox �Ĵ�����е�ʲô�ط���������ǰ����½����Ƿ����κ��������ر����Ĵ��룿

�����ҷ��ֵģ���Ȼ�أ�����ʱ����ĺ�����`run()`.��**����ʱ��**��ָ��ĳ�������������õ��������������ϻ��ѵ���ʱ�䡪���������ú����������ص���ʱ�䡣����Ϊ`run()`����Ҫ���ֽ���ִ��ѭ����������һ�С�

��`run()`�ڲ������ֽ���switch����case�£���һЩС��ʱ�����ڳ���ָ���`OP_POP`,`OP_RETURN`��`OP_ADD`������ص�ָ��`OP_GET_GLOBAL`ռִ��ʱ��� 17%��`OP_GET_PROPERTY`ռ12%��`OP_INVOKE`ռ������ʱ��� 42%��

��������Ҫ�Ż������ȵ㣿ʵ���ϣ�������Ϊ��ʵ֤����������ָ�������ʱ�䶼�����˶�ͬһ�����ĵ����У�`tableGet()`�ú���ռ�������� 72% ��ִ��ʱ�䣨ͬ���������ڣ������ڣ��ڶ�̬���������У��������������൱���ʱ���ڹ�ϣ���в������ݡ������Ƕ�̬�Ĵ��ۡ����ǣ���Ȼ��*�ۡ�*

### 30.2.1 ������װ

������鿴`tableGet()`�����ᷢ������Ҫ�Ƕ�ʵ�ʹ�ϣ�����ҷ���λ�õ� `findEntry()`���õİ�װ��Ϊ��ˢ�����ļ��䣬�����������ģ�

```c
static Entry* findEntry(Entry* entries, int capacity,
                        ObjString* key) {
  uint32_t index = key->hash % capacity;
  Entry* tombstone = NULL;

  for (;;) {
    Entry* entry = &entries[index];
    if (entry->key == NULL) {
      if (IS_NIL(entry->value)) {
        // Empty entry.
        return tombstone != NULL ? tombstone : entry;
      } else {
        // We found a tombstone.
        if (tombstone == NULL) tombstone = entry;
      }
    } else if (entry->key == key) {
      // We found the key.
      return entry;
    }

    index = (index + 1) % capacity;
  }
}
```

������֮ǰ�Ļ�׼����ʱ�����������ҵĻ����ϡ���VM�� 70% ����ִ��ʱ�仨�����������һ���ϡ��ܲ³�����һ���𣿲����������ģ�

```c
uint32_t index = key->hash % capacity;
```

ָ�������ò������⡣�����С`%`����ʵ֤��ģ�����*��ĺ�*����������������������öࡣ���������ø�����

> ��ˮ��ʹ�ú���̸�۵��� CPU ָ������ܣ���Ϊ����������һ�£�������ȡģ�� x86 �ϵļӷ��ͼ�������Լ 30-50����

��һ������£������Ա� CPU ��������ķ�ʽ���û�����������ʵ�ֻ���������������Ͼ������ǵ� C �������ջ����� CPU �Լ����������㡣��������ǿ�����������ļ��ɣ�оƬ�����ʹ�������ˡ�

Ȼ�������ǿ����������Ǳ� CPU ���˽����ǵ�������һ��ʵ������������ʹ��ģ������ȡ�ؼ��ַ����Ĺ�ϣ�벢�����װ���ʺϱ���Ŀ����ķ�Χ��������Ӱ˸�Ԫ�ؿ�ʼ��ÿ����������������֪�������� CPU �� C ��������֪���������ǵı��Ĵ�С���� 2 ���ݡ�

��Ϊ�����Ǵ�����bit��Ū�ߣ���������֪��һ�ָ���ķ���������һ������ 2 ����ȡģ��������**λ����**����������Ҫ���� 229 d��64��ģ������ 37������ʮ�����в����ر����ԣ����ǵ����Զ����Ʋ鿴��Щ����ʱ��������

![](./assets/a0a89e30efdaf1092501a4624b32389477a38d46.png)

�ڲ�ͼ����࣬ע���� (37) �� ������ (229)  �򵥵�ȥ����ߵ���λ�Ľ�� ����ߵ���λ���ڱ�������Ψһ��1����ࡣ

���Ҳ࣬����ȡ��  �� 229 �� 63 ��λAND����õ���ͬ�Ľ�����������ԭ���ĳ�������Сһ���� 2 �����м�ȥ 1 �õ�һϵ�� 1 λ������������Ϊ��ȥ������ߵ�����λ����Ҫ�����롣

���仰˵��������һ������ 2 �������ݵ�ģ ,����ͨ���򵥵� ͨ  2 �������ݼ� 1 ����AND ����ȡ ���Ҳ���һ���㹻����ѧ��������*֤��*���ǿ��еģ����������ϸ���룬��Ӧ�����е����ġ����ǿ����÷ǳ���ĵݼ��Ͱ�λ���滻�Ǹ�����ģ�����������ֻ�轫������Ĵ����и���Ϊ��

```c
static Entry* findEntry(Entry* entries, int capacity,
                        ObjString* key) {
  uint32_t index = key->hash & (capacity - 1);
  Entry* tombstone = NULL;
// table.c, in findEntry(), replace 1 line
```

CPU ϲ����λ���������˺��Ѷ�����иĽ���

> ��һ��Ǳ�ڵĸĽ���ͨ��ֱ�Ӵ洢λ��������������������ݼ������ҵĲ����У���û��ʲô��ͬ����� CPU �������ط�����ƿ����ָ����ˮ�߿�����һЩ������������ѡ�

���ǵ�����̽������������Ҫ���������ĩβ����˻�����һ��ģ��`findEntry()`Ҫ���¡�

```c
      // We found the key.
      return entry;
    }

    index = (index + 1) & (capacity - 1);
  }
// table.c, in findEntry(), replace 1 line
```

����û�г�����̽�����У���Ϊ�����������û�л��С�

��`findEntry()`������һ����ú�����`tableFindString()`����פ��(intern)�ַ������й�ϣ�����ҡ�����Ҳ����������Ӧ����ͬ���Ż���ֻ����פ��(intern)�ַ���ʱ�Ż���ô˺����������ǵĻ�׼���Բ�δ�Դ�ʩ�Ӻܴ�ѹ�������Ǵ��������ַ����� Lox ������ܻ����������ڴ˸��ġ�

```c
  if (table->count == 0) return NULL;

  uint32_t index = hash & (table->capacity - 1);
  for (;;) {
    Entry* entry = &table->entries[index];
// table.c, in tableFindString(), replace 1 line
```

�Լ�������̽�����ʱ��

```c
      return entry->key;
    }

    index = (index + 1) & (table->capacity - 1);
  }
// table.c, in tableFindString(), replace 1 line
```

�����ǿ����޸��Ƿ�ֵ�á��ҵ����˶���ѧ��׼�Լ����������� 10 �������ж������ε� 10,000 �ε��á��������ε��ڸ�������ܡ�����ʹ��δ�Ż�����Ļ����ϣ���׼����ͨ���� 3,192 �����Ρ��Ż������� 6,249��

![](./assets/c364b19018f337435975a8f472997a727729da7c.png)

����ͬ��ʱ���ڣ��⼸���ǹ�����������������ʹ VM ���ٶ������һ����ͨ���ľ��棺�ڴ˻�׼�����У������Ż����棬����һ���޴��ʤ����������������������ץס�����ٷֵ㣬ͨ�����о��ܺá����ڷ������ֶκ�ȫ�ֱ����� Lox �����зǳ��ձ飬�������΢С���Ż�����ȫ��������ܡ�����ÿ�� Lox �ƻ��������档

> ��������Ļ�׼���Թ̶���*������*��Ȼ�������*ʱ��*�����Ľű��Լ������� 10 ���ڿ���ִ�ж��������ÿ���ȷ��ʱ�䲢�������������������ܱȽϣ���ϲ����һ�ֺ�����׼����Ϊ��������ִ���*�ٶ�*��������ֱ�ӱȽ��Ż�ǰ������֡��ڲ���ִ��ʱ��ʱ����������һЩ����������ܻ�����õ�������Բ���ֵ��

���ڣ����ڵ��ص�*������*˵ģ������Ǽ���а��ģ���Ӧ�ô����д��ÿ�������жž�����΢���Ż�Ҳ����һ����Ҫ�Ĺ��̼��ܡ�������������������խ����Ч�Ľ�����������Ǻ����ˡ�

�ؼ��ǣ��ڷ�������������֮ǰ�����ǲ�*֪��*ģ��������������ܡ���������� VM �Ĵ�������Ĵ��߶���äĿ�ز²��ȵ㣬���ǿ��ܲ���ע�⵽���������������׵��ǣ�����Ĺ�������ӵ��һ��̽�����Ƕ�ô��Ҫ��

Ϊ��ǿ����һ�㣬�����Ǽ��������������Ż��� VM ������ԭʼ��׼���ԣ�����������������չʾ��ʲô�����ҵĻ����ϣ�`tableGet()`��Ȼ���൱���ִ��ʱ�䡣���ڶ�̬���͵�������˵�����ǿ���Ԥ�ϵġ������Ѵ�ռ��ִ��ʱ��� 72% �½��� 35%�������������ϣ��������������������ǵ��Ż�����ʹ������죬������*����Ԥ�ڵķ�ʽ*ʹ����졣̽����������֤��������ͷ�������һ�����á�

## 30.3 NaN Boxing

��һ���Ż���һ���ǳ���ͬ�ĸо���ֵ�����ҵ��ǣ��������ֺ���֣����������漰�������ĸ�����ǲ�ͬ�ģ�����û��*��ô*��ͬ��ͨ������֮ǰ���Ż�������������������������������ֻ��Ҫ����һЩ����������������������

�����Ż�����΢�������Ӱ��������������и��ӷ�ɢ��̽������������������������෴������������˼�������ܹ���Ͳ���˷����ġ�

> �Ҳ�ȷ����˭��һ����������Ϸ�ġ������ҵ������������� David Gudeman 1993 ������ġ�Representing Type Information in Dynamically Typed Languages���������˶���������һ�㡣���ŵ����Լ�˵����ƪ���Ĳ�����С˵��Ʒ�����ǡ��㼯�˴�����䴫˵����
> 
> Ҳ���������Ѿ���ʧ��ʱ��������У��������Ѿ������·����˺ܶ�Ρ��κ��� IEEE 754 ��˼���㹻�����˶����ܿ�ʼ���ǳ��Խ�һЩ���õĶ�������������Щδʹ�õ� NaN λ�С�

���������˵�������Ż���Ϊ**NaN װ�� (?NaN boxing)** ����ʱ ��Ϊ**NaN ���(NaN tagging)**���Ҹ���ϲ����һ�����ƣ���Ϊ��װ�䡱������ʾĳ�ֶѷ����ʾ��ʽ����ǰ���ƺ���ʹ�ø��㷺��������ּ����ı��������� VM �б�ʾֵ�ķ�ʽ��

�� 64 λ�����ϣ����ǵ� Value ����ռ�� 16 ���ֽڡ��ýṹ�������ֶΣ�һ�����ͱ�Ǻ�һ��������Ч���ص�union��union�������ֶ���һ�� Obj ָ���һ�� double������ 8 ���ֽڡ�Ϊ��ʹunion�ֶ��� 8 �ֽڱ߽���룬������Ҳ�ڱ�Ǻ���������䣺

![](./assets/4b9d61e0ad227840a1c99401d71aa6217488390a.png)

�����൱��ġ�������ǿ��Լ���������ô VM ���Խ�����ֵ�������ͬ�������ڴ��С����ڴ��������������㹻�� RAM�����ֱ�ӽ�ʡ�ڴ沢����ʲô�����⡣���ǽ�С�ı�ʾ��ζ�Ÿ����ֵ�ʺϻ����С�����ζ�Ÿ��ٵĻ���δ���У����Ӱ��*�ٶ�*��

��� Values ��Ҫ���������Ч���ش�С���룬�� Lox ���ֻ� Obj ָ����Ҫ������ 8 ���ֽڣ�������β��ܱ�ø�С���� Lox �����Ķ�̬���������У�ÿ��ֵ������ҪЯ������Ч���أ���**��ҪЯ���㹻�ĸ�����Ϣ��ȷ������ʱֵ������**�����һ�� Lox �����Ѿ�ʹ���������� 8 ���ֽڣ����ǿ��������ﴢ��һЩ�����λ����������ʱ������һ�����֡���

���Ƕ�̬���Ժڿͳ��ڴ��ڵ�����֮һ�����ر��������ǣ���Ϊ��̬��������ͨ��û��������⡣ÿ��ֵ�������ڱ���ʱ������֪�ģ����������ʱ����Ҫ������ڴ����������������� C ���������� 32 λ int ʱ�����ɵı���*ǡ��*���32 λ�洢�ռ䡣

��̬���Ե��������ھ�̬��Ӫ��ʧ����������������˺ܶ�ǳ������ķ�������������Ϣ����Ч�غɴ���������ı��ء�NaN boxing ��������֮һ�����ر��ʺ� JavaScript �� Lua �����ԣ������������ֶ���˫���ȸ�������Lox Ҳ��ͬһ�����ϡ�

### 30.3.1 ʲô�ǣ����ǣ����֣�

�����ǿ�ʼ�Ż�֮ǰ����Ҫ�����˽� CPU ��α�ʾ�����������켸�����еĻ�����ʹ����ͬ�ķ����������ڹ��ϵľ���[IEEE 754](https://en.wikipedia.org/wiki/IEEE_754)�У����˳�Ϊ����������� IEEE ��׼����

�����ļ�������У�һ��64 λ��˫���ȡ�IEEE ��������������������

> һ�������кܶ����ַ���

![](./assets/3352a8933bf2b3972bda4ec01b5335fb67283eb7.png)

- ���ұ߿�ʼ��ǰ 52 λ��**fraction**��**mantissa**��**significand**λ�����ǽ����ֵ���Ч���ֱ�ʾΪ������������

- �Ա��� 11 ��**ָ��**λ����Щ������β����С���㣨�ţ������ƣ���ƫ���˶�Զ��

- ���λ��**����λ**����ʾ��������������Ǹ�����

��֪�����е㺬���������²����ǶԸ����ʾ������̽�֡��������֪��ָ����β�����һ�𷢻����ã������Ѿ��б���д�ĸ��õĽ����ˡ�

> ���ڷ���λʼ�մ��ڣ���ʹ����Ϊ�㣬��Ҳ��ζ�š����㡱�͡����㡱���в�ͬ��λ��ʾ��ʽ����ʵ�ϣ�IEEE 754 ȷʵ���������ǡ�

�������ǵ�Ŀ�Ķ��ԣ���Ҫ�Ĳ����ǹ淶�ƶ���һ������ָ������������**����ָ��λ**ʱ����ֵ��������һ���ǳ�������֣����Ҿ��в�ͬ�ĺ��塣��Щֵ�ǡ�Not a Number���������**NaN**��ֵ�����Ǵ��������������Ľ���ȸ��

����β��λ��Σ�ָ��λ**��**�����õ��κ�˫���������� NaN������ζ����������಻ͬ��NaN λģʽ��IEEE 754 �����Ƿ�Ϊ���ࡣ���β��λΪ 0 ��ֵ��Ϊ**�ź� NaN (signalling NaNs)**��������ֵ��Ϊ**���� NaN (quiet NaNs)**���ź� NaN ּ����Ϊ�������Ľ�������类�����оƬ���Լ���ʱ������Щֵ֮һ����ȫ��ֹ��������������Ķ����ǣ����ǿ��ܻ����һ���

> �Ҳ�֪���Ƿ����κ� CPU ʵ����*��*���� NaN �źŲ���ֹ���淶ֻ��˵����*����*��

Quiet NaN Ӧ��ʹ����������ȫ�����ǲ��������õ���ֵ��������Ӧ�ò���������������ʱ���������Ż�

����������ָ��λ�����������β��λ��ÿ�� double ����һ�������� NaN�������� 52 λ���䲻�������ǽ���������һ�����������ǾͲ���ȵ� Intel �ġ�QNaN ���㲻ȷ����ֵ������ 51 λ����Щʣ���λ�������κζ���������˵���� 2,251,799,813,685,248 �����صİ��� NaN λģʽ��

![](./assets/1be798d884082862f21e96cd466419d8e4b8389a.png)

����ζ�� 64 λ˫���������㹻�Ŀռ����洢���и��ֲ�ͬ�����ָ���ֵ������*����*�ռ��������ǿ�������ʹ�õ����� 51 λ���ݡ������㹻�Ŀռ���Ԥ������λģʽ����ʾ Lox ��`nil`��`true`��`false`ֵ������ Obj ָ���أ�ָ�벻��Ҫ������ 64 λ��

���˵��ǣ����ǻ�����һ�����С��ǵģ��Ӽ����Ͻ���64 λ�ܹ��ϵ�ָ���� 64 λ�ġ����ǣ�����֪������ϵ�ṹʵ����û��ʹ��������ַ�ռ䡣�෴��������㷺ʹ�õ�оƬֻʹ�õ�48λ������ 16 λδָ����ʼ��Ϊ�㡣

> 48 λ����Ѱַ 262,144 GB ���ڴ档�ִ�����ϵͳ��Ϊÿ�������ṩ���Լ��ĵ�ַ�ռ䣬���Ӧ���㹻�ˡ�

��������� 51 λ�����ǿ������������һ�� 48 λָ�룬������ 3 λ������λ�պ����Դ洢΢С�����ͱ�ǩ������`nil`������ֵ�� Obj ָ�롣

����� NaN boxing���ڵ��� 64 λ˫�������У������Դ洢���в�ͬ�ĸ�����ֵ��ָ�����������������ֵ�е��κ�һ�������ǵ�ǰ Value �ṹ���ڴ�ʹ�������룬ͬʱ�������б���ȡ�

���ֱ�ʾ���ر�õĵط����ڲ���Ҫ*��*����˫����ֵת��Ϊ��boxed����ʽ��Lox ����*ֻ��*��ͨ�� 64 λ˫��������������Ȼ��Ҫ��ʹ������֮ǰ*���*���ǵ����ͣ���Ϊ Lox �Ƕ�̬���͵ģ��������ǲ���Ҫ�����κ�λ�ƻ�ָ�������ӡ�ֵ���������֡���

��������ֵ���ͣ���Ȼ��һ��ת�����衣���ǣ����˵��ǣ����ǵ� VM �����˴�ֵ��ԭʼ���͵����л��ƣ���Щ����������һЩ����档��д��Щ��ʵ�� NaN boxing��VM �����ಿ��Ӧ�ÿ�������������

### 30.3.2 ������֧��

��֪���������������±�ʾ��ϸ�ڡ����ģ����ǻ�������ʵʩ�����о��廯���ڿ�ʼ֮ǰ�����ǽ�����һЩ����ʱ���ּܡ�

��������֮ǰ���Ż���������д��֮ǰ�Ƚ����Ĵ��룬������ˡ�����е㲻ͬ��NaN װ��������оƬ��α�ʾ��������ָ���һЩ�ǳ��ͼ���ϸ�ڡ���*����*�����������������Ĵ���� CPU��������Զ�޷���ȫȷ����

������ǵ� VM ������Ϊ����ֵ��ʾ����ȫʧȥ�Լܹ���֧�֣��Ǿ�̫����ˡ�Ϊ����������������ǽ�����֧��Value�� old tagged union ʵ�ֺ��µ� NaN boxing��ʽ������ʹ�ô˱�־�ڱ���ʱѡ��������Ҫ�ı�ʾ��ʽ��

```c
#include <stdint.h>

#define NAN_BOXING
#define DEBUG_PRINT_CODE
// common.h
```

����Ѷ��壬VM ��ʹ���µ���ʽ�����������ָ�Ϊ����ʽ��ֻ���ע ֵ��ʾϸ�ڵļ��δ��롪����Ҫ��һЩ���ڰ�װ�ͽ��ֵ�ĺꡪ�������Ƿ������������־��������ͬ��VM �����ಿ�ֿ��Լ��������ķ�ʽ��

�󲿷ֹ��������ڡ�value��ģ���У�����������Ϊ������������һ�����֡�

```c
typedef struct ObjString ObjString;

#ifdef NAN_BOXING

typedef uint64_t Value;

#else

typedef enum {
// value.h
```

������ NaN װ��ʱ��ֵ��ʵ��������һ����ƽ���޷��� 64 λ���������ǿ��Ը��� double���⽫ʹ���� Lox ���ֵĺ����һЩ���������������궼��Ҫ���а�λ���㣬�� uint64_t ��һ�ָ��Ѻõ����͡������ģ��֮�⣬VM �����ಿ�ֲ��������������ֻ����ַ�ʽ��

�����ǿ�ʼ����ʵ����Щ��֮ǰ�����ǹرվɱ�ʾ����ĩβ��`#ifdef`�� `#else`��֧ ��

```c
#define OBJ_VAL(object)   ((Value){VAL_OBJ, {.obj = (Obj*)object}})

#endif

typedef struct {
// value.h
```

����ʣ�µ�����ֻ�Ǽ򵥵���`#ifdef`��һ���������е��������ݵ�`#else`��ʵ��������һ���֡����ǽ�һ�δ���һ��ֵ���ͣ�����򵥵����ѡ�

### 30.3.3 ����

���ǽ������ֿ�ʼ����Ϊ������ NaN boxing�¾�����ֱ�ӵı�ʾ��Ҫ�� C double��ת����Ϊ NaN boxing�� clox ֵ�����ǲ���Ҫ�����κ�һλ����ʾ��ȫ��ͬ����������ȷʵ��Ҫ�����ǵ� C ���������������ʵ������ͨ���� Value ����Ϊ uint64_t ��ʹ����ø��ѡ�

������Ҫ�ñ�������ȡһ������Ϊ��˫���ȵ�λ��������Щλ���� uint64_t����֮��Ȼ�����Ϊ**����˫��(type punning)**�������ȿ�� 8 ��ʱ��������C �� C++ ����Աһֱ���������������Թ淶��ԥ�������޷�˵���ٷ��ϿɵĶ��ַ����е���һ�֡�

> �淶���߲�ϲ������˫�أ���Ϊ��ʹ�Ż���ø������ѡ�һ��ؼ����Ż���������������ָ������� CPU ��ִ�йܵ�����Ȼ��������ֻ������������������û��ɼ���Ч��ʱ��������������롣
> 
> ָ��ʹ����ѡ��������ָ��ָ����ͬ��ֵ�����ܶ�һ��д�����һ����ȡ�����������򡣵�������*��ͬ*���͵�ָ���أ�������ǿ���ָ��ͬһ��������ô������*�κ�*����ָ�붼������ͬһ��ֵ�ı������⼫��������˱��������������������еĴ�������
> 
> Ϊ�������������������ϣ������**�ϸ�ı���**�������������͵�ָ�벻��ָ����ͬ��ֵ������˫�ر����ϴ�����������衣

��֪��һ�ֽ� `double`����ת��`Value`�ķ����������� C �� C++ �淶��֧�����ַ��������ҵ��ǣ������ʺϵ�������ʽ�����ת���������ø������������ǵ�һ���꣺

```c
typedef uint64_t Value;

#define NUMBER_VAL(num) numToValue(num)

#else
// value.h
```

�ú������ﴫ����˫����ֵ��

```c
#define NUMBER_VAL(num) numToValue(num)

static inline Value numToValue(double num) {
  Value value;
  memcpy(&value, &num, sizeof(double));
  return value;
}

#else
// value.h
```

��֪��������֣��԰ɣ���һϵ���ֽ���Ϊ���в�ͬ���Ͷ����ı����ǵ�ֵ�ķ�����`memcpy()`���⿴�������ÿ��£�����һ���ֲ�������ͨ��ϵͳ���ý����ַ���ݸ�����ϵͳ�Ը��Ƽ����ֽڡ�Ȼ�󷵻ؽ��������������ֽ���ȫ��ͬ��ֵ�����ҵ��ǣ���Ϊ��*��*֧������˫�ص�ϰ���÷������Դ��������������ʶ���ģʽ��`memcpy()`��ȫ�Ż�����

��չ����һ�� Lox ���Ǿ���

```c
typedef uint64_t Value;

#define AS_NUMBER(value)    valueToNum(value)

#define NUMBER_VAL(num) numToValue(num)
// value.h
```

�ú���ô˺�����

```c
#define NUMBER_VAL(num) numToValue(num)

static inline double valueToNum(Value value) {
  double num;
  memcpy(&num, &value, sizeof(Value));
  return num;
}

static inline Value numToValue(double num) {
// value.h
```

���Ĺ���ԭ����ȫ��ͬ��ֻ�����ǽ��������͡�ͬ����������������������Щ����ʹ��Щ����`memcpy()`����ʧ��������Ȼ��Ҫ��ʾ�������ڵ��õı��룬`memcpy()`�������ǻ���Ҫһ��include��

> ����������Լ�ʹ�õı������޷��Ż�`memcpy()`���볢�����²�����
> 
> ```c
> double valueToNum(Value value) {
>   union {
>     uint64_t bits;
>     double num;
>   } data;
>   data.bits = value;
>   return data.num;
> }
> ```

```c
#define clox_value_h

#include <string.h>

#include "common.h"
// value.h
```

���Ǻܶ���룬����ʲô��������ֻ���� C ���ͼ������������ Lox ��Ž�������ʱ����*����*�����ȤһЩ�����������ӵ�е�ֻ��һ�� double ��λ����������ж���*��*һ�� double��ʱ����ת�ˡ�

```c
typedef uint64_t Value;

#define IS_NUMBER(value)    (((value) & QNAN) != QNAN)

#define AS_NUMBER(value)    valueToNum(value)
// value.h
```

����֪����ÿ��*����*���ֵ�ֵ����ʹ������İ��� NaN ��ʾ�����Ǽ��������Ѿ���ȷ�ر������κο���ͨ�������ֽ���������ʵ�ʲ������������ NaN ��ʾ��

��� double ���������� NaN λ������������ quiet NaN λ������Ϊ�˸��õĺ�����׼��������һ�������ǿ��Էǳ�ȷ�����������Լ�Ϊ��������Ԥ����λģʽ֮һ��Ϊ�˼����һ�㣬���������˳������ǵ�һ�鰲���� NaN λ֮�������λ�����*����*��Щλ�������ã��������������� Lox ���͵� NaN boxingֵ��������ʵ������һ�����֡�

> �൱ȷ�����������ϸ�֤��������֪��û��ʲô����ֹ CPU ���� NaN ֵ��ΪĳЩ�����Ľ������λ��ʾ������������λ��ʾ��ͻ���������Ҷ�����ܹ��Ĳ����У��һ�û�п�����������

һ�鰲���� NaN λ�������£�

```c
#ifdef NAN_BOXING

#define QNAN     ((uint64_t)0x7ffc000000000000)

typedef uint64_t Value;
// value.h
```

��� C ֧�ֶ��������־ͺ��ˡ����ǣ����������ת�������ῴ����ֵ�����ͬ��

![](./assets/4c88f90bc6744b2ac45ab97fb0bf59e5864f3cd9.png)

���������е�ָ��λ�����ϰ����� NaN λ���ټ���һ�������λ���ܿ� Intel ֵ��

### 30.3.4 Nil, true, and false

��һ��Ҫ������������`nil`����ܼ򵥣���Ϊֻ��һ��`nil`ֵ���������ֻ��Ҫһ��λģʽ����ʾ��������������������ֵ����������ֵ��`true`��`false`������Ҫ�ܹ��������ص�λģʽ��

��λ�����������ֲ�ͬ����ϣ����Ѿ��㹻�ˡ����ǽ�δʹ�õ�β���ռ���������λ����Ϊ�����ͱ�ǩ������ȷ���������ڲ鿴����������ֵ�е���һ�����������ͱ�ǩ�������£�

```c
#define QNAN     ((uint64_t)0x7ffc000000000000)

#define TAG_NIL   1 // 01.
#define TAG_FALSE 2 // 10.
#define TAG_TRUE  3 // 11.

typedef uint64_t Value;
// value.h
```

��ˣ����ǵı�ʾ`nil`�Ƕ������ǵİ��� NaN ��ʾ���������λ�Լ�`nil`���ͱ��λ��

![](./assets/2ced54d6aedc37ebb172c56f1c5ac8bc4969c592.png)

�ڴ����У����������������Щλ��

```c
#define AS_NUMBER(value)    valueToNum(value)

#define NIL_VAL         ((Value)(uint64_t)(QNAN | TAG_NIL))
#define NUMBER_VAL(num) numToValue(num)
// value.h
```

���Ǽ򵥵ذ�λOR������ NaN λ�����ͱ�ǣ�Ȼ����һ��С��Ա�赸������ C ������������Ҫ��Щλ��ʾʲô��

����`nil`ֻ��һλ��ʾ�����ǿ����� uint64_t ��ʹ����������鿴 Value �Ƿ�Ϊ`nil`��

```c
typedef uint64_t Value;

#define IS_NIL(value)       ((value) == NIL_VAL)
#define IS_NUMBER(value)    (((value) & QNAN) != QNAN)
// value.h
```

����Բµ���������ζ���`true`��`false`ֵ�ġ�

```c
#define AS_NUMBER(value)    valueToNum(value)

#define FALSE_VAL       ((Value)(uint64_t)(QNAN | TAG_FALSE))
#define TRUE_VAL        ((Value)(uint64_t)(QNAN | TAG_TRUE))
#define NIL_VAL         ((Value)(uint64_t)(QNAN | TAG_NIL))
// value.h
```

��Щλ��������������

![](./assets/ff57b3275d235ece1bef4a90ae0abcc4b3c2a848.png)

Ҫ�� C bool ת��Ϊ Lox Boolean����������������������ֵ�����õľ������������

```c
#define AS_NUMBER(value)    valueToNum(value)

#define BOOL_VAL(b)     ((b) ? TRUE_VAL : FALSE_VAL)
#define FALSE_VAL       ((Value)(uint64_t)(QNAN | TAG_FALSE))
// value.h
```

������һ�ָ������İ�λ��ʽ��������һ�㣬���ҵ�ֱ���Ǳ��������Ա��Ҹ�����ҳ�һ����ת����һ��������򵥡�

```c
#define IS_NUMBER(value)    (((value) & QNAN) != QNAN)

#define AS_BOOL(value)      ((value) == TRUE_VAL)
#define AS_NUMBER(value)    valueToNum(value)
// value.h
```

��Ϊ����֪���� Lox ��ǡ������������λ��ʾ���������� C �У��κη���ֵ�����Ա���Ϊ�ǡ��桱�������������`true`����һ����`false`���˺�ȷʵ������������֪��*��*Lox ����ֵ��ֵ�ϵ�������Ϊ�˼����һ�㣬����һ���ꡣ

```c
typedef uint64_t Value;

#define IS_BOOL(value)      (((value) | 1) == TRUE_VAL)
#define IS_NIL(value)       ((value) == NIL_VAL)
// value.h
```

�⿴�����е���֡�һ�������Եĺ꿴������������

```c
#define IS_BOOL(v) ((v) == TRUE_VAL || (v) == FALSE_VAL)
```

���ҵ��ǣ��Ⲣ����ȫ����չ���ᵽ`v`�����Σ�����ζ������ñ���ʽ���κθ����ã����ǽ���ִ�����Ρ����ǿ����ú����һ�������ĺ��������ǣ��������鷳����

�෴�����Ƕ�ֵ���а�λ��1 �Ժϲ����е�������Ч����λģʽ����������ֵ���Դ��ڵ�����Ǳ��״̬��

1. ������`FALSE_VAL`���������Ѿ�ת��Ϊ`TRUE_VAL`.

2. ��ȥ��`TRUE_VAL`��`| 1`ʲôҲû����������Ȼ��`TRUE_VAL`��

3. ��������һЩ�ǲ���ֵ��

��ʱ�����ǿ��Լ򵥵ؽ������`TRUE_VAL`���бȽϣ����������Ǵ���ǰ����״̬���ǵ�����״̬��

### 30.3.5 ����

���һ��ֵ���������ѵġ��뵥��ֵ��ͬ��������Ҫ����ʮ�ڸ���ͬ��ָ��ֵװ�� NaN �С�����ζ�����Ǽ���Ҫĳ�ֱ����ָʾ��Щ�ض��� NaN*��*Obj ָ�룬Ҳ��ҪΪ��ַ���������ռ䡣

�������ڵ���ֵ�ı��λλ���Ҿ����洢ָ�뱾��������������ǲ������׵�������ʹ�ò�ͬ��λ��ָʾ��ֵ��һ���������á�Ȼ��������һ������û��ʹ�á������������е� NaN ֵ���������֡��������������С�������λ�������κ����顣���ǽ�����ʹ������Ϊ��������ͱ�ǩ��������ǵ�һ�� quiet NaN �����˷���λ����ô������һ�� Obj ָ�롣��������������ǰ�ĵ���ֵ֮һ��

> ʵ���ϣ���ʹֵ�� Obj ָ�룬����Ҳ*����ʹ�����λ���洢���ͱ�ǡ�*������Ϊ Obj ָ�������� 8 �ֽڱ߽���룬��Ϊ Obj ����һ�� 64 λ�ֶΡ�������������ζ�� Obj ָ����������λ��ʼ��Ϊ�㡣���ǿ��������д洢������Ҫ���κ����ݣ�����ȡ������ָ��֮ǰ�������ε���
> 
> ������һ��ֵ��ʾ�Ż�����Ϊ**ָ����**��

��������˷���λ����ʣ��ĵ�λ�洢ָ�� Obj ��ָ�룺

![](./assets/43601b65bd0104665166ef74d74db8406cae9ea6.png)

Ҫ��ԭʼ Obj ָ��ת��Ϊֵ�����ǻ�ȡָ�벢�������а����� NaN λ�ͷ���λ��

```c
#define NUMBER_VAL(num) numToValue(num)
#define OBJ_VAL(obj) \
    (Value)(SIGN_BIT | QNAN | (uint64_t)(uintptr_t)(obj))

static inline double valueToNum(Value value) {
// value.h
```

ָ�뱾����һ�������� 64 λ��ԭ���ϣ������������һЩ������ NaN �ͷ���λ�ص�����ʵ���ϣ��������Ҳ��Թ�����ϵ�ṹ�У�ָ���е� 48 λ���ϵ���������ʼ��Ϊ�㡣��������˺ܶ�ת�����ҷ�����������һЩ�����޵� C ������������ģ������ս��ֻ�ǽ�һЩλƴ����һ��

> ���漰�������еĴ���ʱ���Ҿ�����ѭ���ɵ����ģ�������һ���ǿ��ɵġ����Ż�ʱ��������Ҫͻ��*�淶��˵*�Ŀ����������飬��Ҫͻ�������ı�������оƬ�������������Ľ��ޡ�
> 
> �����淶���з��գ������޷����������Ҳ�лر����������������Ƿ�ֵ�á�

���������������λ��

```c
#ifdef NAN_BOXING

#define SIGN_BIT ((uint64_t)0x8000000000000000)
#define QNAN     ((uint64_t)0x7ffc000000000000)

// value.h
```

Ҫ�ָ� Obj ָ�룬����ֻ�����ε�������Щ�����λ���ɡ�

```c
#define AS_NUMBER(value)    valueToNum(value)
#define AS_OBJ(value) \
    ((Obj*)(uintptr_t)((value) & ~(SIGN_BIT | QNAN)))

#define BOOL_VAL(b)     ((b) ? TRUE_VAL : FALSE_VAL)
// value.h
```

���˺� (`~`)�������֮ǰû�������㹻���λ����������������λȡ�������л���������е����� 1 �� 0��ͨ���� quiet NaN �ͷ���λ�İ�λ��������ֵ������*���*����Щλ����ָ��λ������

���һ���꣺

```c
#define IS_NUMBER(value)    (((value) & QNAN) != QNAN)
#define IS_OBJ(value) \
    (((value) & (QNAN | SIGN_BIT)) == (QNAN | SIGN_BIT))

#define AS_BOOL(value)      ((value) == TRUE_VAL)
// value.h
```

�洢 Obj ָ���ֵ�����������λ�����κθ���Ҳ����ˡ�Ҫ�ж�һ�� Value �Ƿ���һ�� Obj ָ�룬������Ҫ������λ�����а����� NaN λ�Ƿ������á������������Ǽ�ⵥ��ֵ���͵ķ�ʽ��ֻ���������ʹ�÷���λ��Ϊ��ǡ�

### 30.3.6 ֵ���� value functions

VM �����ಿ����ʹ��ֵʱͨ����ͨ���꣬�������Ǽ�������ˡ�Ȼ������value��ģ�����м����������Կ��� Value �������ں��Ӳ�ֱ��ʹ�����ı��롣����Ҳ��Ҫ�����Щ���⡣

��һ����`printValue()`��ÿ��ֵ���Ͷ��е����Ĵ��롣���ǲ����п��Դ򿪵���ʽ����ö�٣��������ʹ��һϵ�����Ͳ���������ÿ��ֵ��

```c
void printValue(Value value) {
#ifdef NAN_BOXING
  if (IS_BOOL(value)) {
    printf(AS_BOOL(value) ? "true" : "false");
  } else if (IS_NIL(value)) {
    printf("nil");
  } else if (IS_NUMBER(value)) {
    printf("%g", AS_NUMBER(value));
  } else if (IS_OBJ(value)) {
    printObject(value);
  }
#else
  switch (value.type) {
// value.c, in printValue()
```

���ڼ����ϱȿ�����һ�㣬����ʵ��д�����Ŀ�����ȣ������Ժ��Բ��ơ�

������Ȼ֧��ԭʼ�ı�����ϱ�ʾ��������Ǳ����ɴ��벢���������`#else`���������С�

```c
  }
#endif
}
// value.c, in printValue()
```

��һ�������ǲ�������ֵ�Ƿ���ȡ�

```c
bool valuesEqual(Value a, Value b) {
#ifdef NAN_BOXING
  return a == b;
#else
  if (a.type != b.type) return false;
// value.c, in valuesEqual()
```

û�б�����򵥵��ˣ��������λ��ʾ��ͬ����ֵ��ȡ���Ե���ֵ��˵����ȷ�ģ���Ϊÿ������ֵ����Ψһ��λ��ʾ����������ֻ���������Լ�����ҲΪ Obj ָ��������ȷ�����飬��Ϊ����ʹ�ñ�ʶ����ʾ��ȡ���ֻ�е����� Obj ����ָ����ȫ��ͬ�Ķ���ʱ���ǲ���ȡ�

����Ҳ*�����*��ȷ�ġ���������в�ͬλ��ʾ�ĸ��������ǲ�ͬ����ֵ��Ȼ����IEEE 754 ����һ���Ӷ��������Ǹе����������Ҳ���ȫ�����ԭ�򣬹淶Ҫ�� NaN values are not equal to themselves����������Ϊ�Լ���Ŀ��ʹ�õ����ⰲ�� NaN���ⲻ�����⡣������ Lox �в���һ���������ġ����� NaN �ǿ��ܵģ������������ȷ��ʵ�� IEEE 754 ���֣���ô���ֵ��Ӧ�õ������������������˵��

```c
var nan = 0/0;
print nan == nan;
```

IEEE 754 ˵�������Ӧ�ô�ӡ��false������ʹ�����Ǿɵı�����ϱ�ʾ����ȷ�����飬��Ϊ����`VAL_NUMBER`�������`==`�� C ������֪����˫���ȵ�����ֵ����ˣ�������������ȷ�� CPU ָ����ִ�� IEEE ������ȡ�

���ǵ��±�ʾͨ���� Value ����Ϊ uint64_t ��������һ�㡣���������*��ȫ*���� IEEE 754��������Ҫ�������������

```c
#ifdef NAN_BOXING
  if (IS_NUMBER(a) && IS_NUMBER(b)) {
    return AS_NUMBER(a) == AS_NUMBER(b);
  }
  return a == b;
// value.c, in valuesEqual()
```

��֪���������֡�ÿ�����Ǽ������ Lox ֵ�Ƿ����ʱ�������������Ͳ��Զ���������ܳɱ����������Ը������һ������ԡ���˭*����*���� NaN �Ƿ񲻵������Լ���-���ǿ��԰����ص����һ������������������ö�ô�ظ���

> ��ʵ�ϣ�jlox Ū���� NaN ����ԡ�����ʹ�� �Ƚ�ԭʼ˫��ʱ��Java ������ȷ������`==`���������������װ�䵽 Double �� Object ��ʹ�� �Ƚ����ǣ�Java �ͻ�����ȷ������`equals()`������� jlox ʵ������Եķ�ʽ��

������ǹر�Χ�ƾ�ʵ�ֵ��������벿�֡�

```c
  }
#endif
}
// value.c, in valuesEqual()
```

��������������Ż�������ˣ����ǵ� clox �����Ҳ����ˡ����������´�������һ�С�

### 30.3.7 ��������

�����Ѿ���ɣ���������Ȼ��ҪŪ��������Ƿ����ͨ����Щ�������˸��õ����顣�����������Ż���֮ǰ���Ż��кܴ�ͬ�������������̽�����п�����һ���������ȵ㡣�����޸����ǲ��ִ��룬�������������ȵ��ø��졣

����ֵ��ʾ��Ӱ����ӷ�ɢ������ʹ�����ǵ��κεط�������չ��������ܱ仯��������������Ժܺø��ٵķ�ʽ�ֲ�������������У����������Ż������С�

> �ڽ��з�������ʱ������������ϣ������������Ż����������汾����Ϊ����ӳ�������û���������ܹ��¡��������Ż�������������������Ӱ��������Щ�����������ȵ㡣�ֶ��Ż����Թ������ܻ������������޸����Ż��������Ѿ�Ϊ����������⡣
> 
> ȷ������������ضԵ��԰汾���л�׼���Ժ��Ż������ƺ�ÿ�����ٷ�һ�������Ĵ���

����Ҳ��������*�ƶ�*�����ǵı仯��Ӱ�졣����ʹֵ��С���Ӷ����������� VM �Ļ���δ�����ʡ����ø��ĵ�ʵ������Ч���ںܴ�̶���ȡ�����������е� Lox ������ڴ�ʹ�������һ��΢С�� Lox ΢��׼���Կ���û���㹻��ֵɢ�����ڴ���������Ч�������ԣ������� C �ڴ�������ַ������ǵĵ�ַ֮��Ķ���Ҳ��Ӱ������

������������˹�����������һ�ж����ø���һЩ���������ڸ��󡢸����ӵ� Lox �����ϡ����ǣ��� NaN װ��ֵʱ������ִ�еĶ��ⰴλ������ܻ�������õ��ڴ�ʹ�ô��������档�������������ܹ������˲�������Ϊ���޷�����*֤��*���Ѿ�ʹ VM ��ø��á��㲻��ָ��һ����һ������Ŀ��΢��׼˵����������������𣿡�

�෴������������Ҫ����*һ��*����Ļ�׼����������£�����Ӧ���Ǵ���ʵ�����Ӧ�ó��������������ġ����������� Lox ������������Դ������������顣Ȼ�����ǿ��Ժ���������Щ��������弨Ч�仯���Ҿ���ƴ����һЩ�ϴ�� Lox �������ҵĻ����ϣ��µ�ֵ��ʾ�ƺ�ʹһ�ж����˴�Լ 10%��

�Ⲣ����һ���޴�ĸĽ�����������ʹ��ϣ�������ٶȸ������ԶӰ����ȡ������Ӵ��Ż��ںܴ�̶�������Ϊ����������������ĳ�����ܹ�����һ���ܺõ����ӣ���ʵ˵����Ϊ����Ϊ���ڼ�������ĺܿᡣ���������س���ʹ clox ���죬����ܲ���������Ҫ�������顣���ܻ�������������ʵ�ֵĳɹ���

���ǣ�����������Լ����µ���Ŀ�Ѿ�ȡ�����������ɵ�ʤ������ô��ĳЩʱ����������Ҫ���ǵ������ļ�ֵ��ʾ����ϣ�������Ѿ����������ڸ������һЩѡ��

## 30.4 ��һ��ȥ����

���ǽ��� Lox ���Ժ����ǵ�������������ֹͣ�����ǿ�����Զ�޲����������µ����Թ��ܺ�������ٶȸĽ������ǣ������Ȿ�飬����Ϊ�����Ѿ���Ȼ��Ȼ����������ǵĹ������Ҳ����ظ������ڹ�ȥ����ҳ��ѧ�����������ݡ��������һ����ǵá��෴�����뻨һ��ʱ��̸̸�����������ȥ������ı������֮�õ���һ����ʲô��

�����еĴ�����˿��ܲ��Ὣְҵ���ĵĴ󲿷�ʱ�仨�ڱ�������������ϡ���ֻռ�������ѧѧ�����һС���֣��ڹ�ҵ������������ռ�ı���������С��û��ϵ����ʹ���Ⱳ����Ҳû��ʹ�ù�����������Ҳһ����*ʹ��*������ϣ���Ȿ�����������õ���������ʹ�õı�������������ƺ�ʵ�ֵġ�

����ѧϰ��һЩ��Ҫ�Ļ������ݽṹ���������һЩ���еͼ��������Ż�������ʵ�������������ĸ������̣�����רҵ֪ʶ��������������

��Ҳϣ���Ҹ�����һ�ֿ����ͽ��������·�������ʹ����Ҳû���о���һ�����ԣ������ܻᾪ�ȵط����ж��ٱ��������Ա���Ϊ������*����*��Ҳ������Ҫ��д�ı������������Խ�ģΪ��������ִ�С���һϵ�л��ڶ�ջ�ġ�ָ�������Ҫ���ֵ��û����濴�����ǳ������ AST��

> ��Ҳ���������������Ҳ���Ϊ���ڱ����ѧ�����κ�һ�����⡪�������ڱ��֮�⡪�������ն������������������á�����ϲ�����������̷���֮һ��������Щ���в���һ����Ȥ���˵Ľ�����

�����ȷʵ�������˽������Ե����Ӷ���������һЩ����̽����������Щ��֧�Ľ��飺

- ���Ǽ򵥵ĵ����ֽ����������������������Ҫ������ʱ�Ż�����һ�����������ʵ���У�����ʱ�Ż�ͨ����Ϊ��Ҫ���������Ż�������ǳ��ḻ������һ������ı������鼮���� clox �� jlox ��ǰ���ؽ�Ϊһ�����ӵı���ܵ������а���һЩ��Ȥ���м��ʾ���Ż����̡�
  
  ��̬���ͻ�������߶�Զʩ��һЩ���ƣ�������Ȼ�������ܶ����顣�����������Ҫ��Ծ������ Lox ���Ӿ�̬���ͺ����ͼ��������϶��������ǰ���и���Ķ������Ծ׽���
  
  ��ϲ�� Cooper �� Torczon ��*Engineering a Compiler* for this��Appel �� *Modern Compiler Implementation*�鼮Ҳ���ܻ�ӭ��

- ���Ȿ���У��ҵ�Ŀ������ȷ���������ر��ϸ��ҵ�Ŀ����Ҫ�������Դ������Թ�����ֱ���͸о��������ϲ������ȷ����ô�����������ѧ���綼�ڵ����㡣���Ժͱ�����������ӵ�м����֮ǰ���Ѿ�����ʽ�о�������˲������ڽ��������ۡ�����ϵͳ���������ʽ�߼����鼮�����ġ���������·����ȥ�����������Ķ� CS ���ģ��Ȿ������һ����м�ֵ�ļ��ܡ�

- ���ߣ���������ϲ���ڿ͹����ʹ������ԣ�������ʹ�� Lox �����������Լ���������﷨����Ϊ������������ݡ�����ȱ�ٵĹ��ܻ�ɾ������ϲ���Ĺ��ܡ�����������µ��Ż���

> �Ȿ������ְ�Ȩ�������У��� jlox �� clox �Ĵ���*��*ʵ��ʹ�÷ǳ����ɵ�*MIT*[����֤](https://en.wikipedia.org/wiki/MIT_License)���ǳ���ӭ��[ѡ�������κ�һλ������](https://github.com/munificent/craftinginterpreters)��������������������Ϊ�顣ȥ���ϡ�
> 
> ����������Խ����ش���ģ����Ҳ�������ƣ���Ҫ��Ϊ�˱���������Ƕԡ�Lox����������������ĺ��塣

 ��������ܻ�ﵽ����һ���ز�������ӵ��һЩ����Ϊ������Ҳ����ʹ�õĶ������⽫��������������*����*�Ľ�Ȼ��ͬ�����硣�������Ѵ���ʱ���д�ĵ���ʾ�����򡢹��ߺ����õĿ⡣����������������û������ԡ�Ҫ�ڸ��������չ�����������Ӫ��ñ�Ӳ�����*����*������ÿ���˶�ϲ�����������ڵĹ������������ϲ������������ʹ����������������Լ���ǳ�������ο��

����Ҳ���Ȿ���Ѿ���������Ŀ��������������ͣ������������������·�����߲�������·���Ҷ�ϣ������һ����ѵ������һ������������ܻᱻ��������ŵ���������Щ�½��У����Ѿ��������������������Ϊ��һ��һ����ӡ����ʹ������������ս�ԵĲ���Ҳ���������Ƿ��˽������������Դ����������ͽ���������ô���������κ������������顣

---

## [��ս](http://craftinginterpreters.com/optimization.html#challenges)

��ѧУ�����һ�첼�ü�ͥ��ҵ�ƺ��ܲпᣬ������������������ڼ�����ʲô��

1. �������ķ����������м�����׼���ԣ����� VM ��Ѱ�������ȵ㡣��������ʱ�����κο��ԸĽ��ĵط���

2. ʵ���û������е������ַ�������С��ͨ��ֻ��һ�����ַ������� clox �в�̫��Ҫ����Ϊ���ǻᱣ���ַ������������ VM ���ᡣ������Щ�����������ˣ���Ϊÿ��С�ַ�������һ��С�ַ����飬Ȼ��ֵ��ʾΪָ��������ָ����һ���˷ѡ�ͨ����ָ����ַ������ַ���һ������ļ�����ΪС�ַ���ʹ�õ�����ֵ��ʾ�����ַ������洢��ֵ�С�
   
   �� clox ��ԭʼ������ϱ�ʾ��ʼ��ʵʩ���Ż�����дһЩ��صĻ�׼���ԣ������Ƿ��а�����

3. �ع�����Ȿ��ľ�����������Щ���ֶ������ã�ʲôû�У����¶��ϻ����϶���ѧϰ������˵�������𣿲�ͼ�а������Ƿ�ɢע��������Щ����ǳ��廹�ǻ�����
   
   ��Խ�˽���ĸ���ѧϰ������Խ����Ч�ؽ�֪ʶ�ϴ������ͷ���С�������ר����Կ��Խ�����õ�ѧϰ��ʽ�Ĳ��ϡ�