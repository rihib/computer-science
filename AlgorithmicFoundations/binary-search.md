# 二分探索の実装

## 半閉区画

### 最初に見つけたtargetのインデックスを返す場合

半閉区画の場合、`right`は常に探索済みの範囲に含める必要がある。そのため範囲外のインデックス（`len(nums)`）で初期化する必要があるし、すでに探索済みの値（`mid`）で更新する必要がある。また`left == right`になるということは`left`も探索済みとなり、全ての探索が終了したことがわかるので、探索を終了する必要がある。

```Go
// 半閉区画（最初に見つけたtargetのインデックスを返す）
// nums := []int{0, 0, 1, 1, 1, 1, 1, 1, 1, 1}; target := 1;のとき、インデックス5が返される
func search(nums []int, target int) int {
  left, right := 0, len(nums)
  for left < right {
    mid := left + (right-left)/2
    if nums[mid] == target {
      return mid
    }
    if target < nums[mid] {
      right = mid
    } else {
      left = mid + 1
    }
  }
  return -1
}
```

`target`未満の値の場合は`false`、`target`以上の値の場合は`true`だとすると、探索対象の数列は`[false, false, ..., true, true, ...]`のようなbool値の集合と捉えることができる。`false`と`true`の境界にいる場合は`left`は`mid + 1`で`false`から`true`に更新される。一方で、`right`は`mid`が`true`の場合に`mid`で更新されるので、常に`true`の範囲内に居続ける（インデックスの範囲外も`true`とみなすなら）し、いきなり`right`が`left`を飛び越える（`right < left`）ことはない。そのため、下記のように書くこともできる。

```Go
// 半閉区画（最初に見つけたtargetのインデックスを返す）
// nums := []int{0, 0, 1, 1, 1, 1, 1, 1, 1, 1}; target := 1;のとき、インデックス5が返される
func search(nums []int, target int) int {
  left, right := 0, len(nums)
  for {
    if left == right {
      break
    }
    mid := left + (right-left)/2
    if nums[mid] == target {
      return mid
    }
    if target < nums[mid] {
      right = mid
    } else {
      left = mid + 1
    }
  }
  return -1
}
```

### 境界の右側を返す場合

ところで同じ二分探索でも、同じ`target`ならどの`target`でも良いのか、それとも最初の`target`（`false`と`true`の境界）を返したいのかで実装が変わってくる。これまでに提示した２つの例はどちらも同じ`target`ならどの`target`でも返して良いという考えのもとつくられた実装である。この場合、例えば`nums := []int{0, 0, 1, 1, 1, 1, 1, 1, 1, 1}; target := 1;`のとき、インデックス5が返される。一方で最初の1（インデックス2）を返して欲しい場合はこれまでの実装では適さない。

最初の1（インデックス2）を返す実装を作りたい場合は、最初の`true`（`false`と`true`の境界）を求めてから、その`true`が`target`かどうかを確かめるという方法を取れば良い。

`right`が`left`を飛び越えることはないので、必ず`left == right`になった瞬間が`false`と`true`の境界だとわかる。`left`が`false`から`true`の境界に入ったときに、`right`が`left < right`の位置にいる可能性はあるが、その場合は必ず`target < nums[mid]`になるので、`left`は動かず、`right`のみが更新されて徐々に`left`に近づいていき、最終的に`left`と`right`は重なる。

`left == right`になったとき、`left`と`right`はともに一番最初にある`true`にいるが、全て`false`の場合は`right`は範囲外のインデックスのまま動かないし、`true`が存在するとしても`target`自体は存在しない可能性が考えられるので、このときの`left`または`right`の値が`target`と一致するかを確認する必要がある。

```Go
// 半閉区画（境界の右側を返す）
// nums := []int{0, 0, 1, 1, 1, 1, 1, 1, 1, 1}; target := 1;のとき、インデックス2が返される
func search(nums []int, target int) int {
  left, right := 0, len(nums)
  for left < right {
    mid := left + (right-left)/2
    if nums[mid] < target {
      left = mid + 1
    } else {
      right = mid
    }
  }
  if left >= len(nums) || nums[left] != target {
    return -1
  }
  return left
}
```

```Go
// 半閉区画（境界の右側を返す）
// nums := []int{0, 0, 1, 1, 1, 1, 1, 1, 1, 1}; target := 1;のとき、インデックス2が返される
func search(nums []int, target int) int {
  left, right := 0, len(nums)
  for left < right {
    mid := left + (right-left)/2
    if nums[mid] < target {
      left = mid + 1
    } else {
      right = mid
    }
  }
  if right >= len(nums) || nums[right] != target {
    return -1
  }
  return right
}
```

## 閉区画

### 最初に見つけたtargetのインデックスを返す場合

対して閉区画の場合は、`right`は常に未探索の範囲に含める必要があるため、`len(nums)-1`で初期化し、`mid + 1`で更新する必要がある。また`right`も`false`と`true`の境界にいる場合は`mid - 1`で`true`から`false`に更新されるため、`left`と`right`は`true`の範囲内（最初の`true`）で重なる場合もあれば、`false`の範囲内（最後の`false`）で重なる可能性もある。`true`の範囲内で重なる場合は、必ず`target < nums[mid]`となるので、`left`は動かず、`right`だけが更新されて`false`の範囲内に入ることになる。反対に`false`の範囲内で重なる場合は、`right`は動かず、`left`が`true`の範囲内に入ることになる。そのため、`left == right`の時はまだ探索を続ける必要があり、`right < left`となったときに初めて探索を終了することができる。

これは考えてみれば当たり前で、閉区画の場合`right`は未探索の範囲に含まれているので、`left == right`の時はまだ未探索の領域が残っているため、探索を続ける必要があると捉えることができ、`right < left`となったときに初めて未探索の領域がなくなるので探索を終了することができるわけである。

ちなみに下記の実装では`nums := []int{0, 0, 1, 1, 1, 1, 1, 1, 1, 1}; target := 1;`のとき、インデックス4が返される。半閉区画と違って`right`が`len(nums)-1`で初期化されているためである。この実装は`target`ならなんでも良いので返すという実装なので、どのインデックスを返そうがあまり大きな違いはない。

```Go
// 閉区画（最初に見つけたtargetのインデックスを返す）
// nums := []int{0, 0, 1, 1, 1, 1, 1, 1, 1, 1}; target := 1;のとき、インデックス4が返される
func search(nums []int, target int) int {
  left, right := 0, len(nums)-1
  for left <= right {
    mid := left + (right-left)/2
    if nums[mid] == target {
      return mid
    }
    if target < nums[mid] {
      right = mid - 1
    } else {
      left = mid + 1
    }
  }
  return -1
}
```

### 境界の右側を返す場合

`right < left`となった瞬間、`right`は`false`の範囲内に入ってしまっているが、`left`は必ず一番最初にある`true`にいる。そのため、下記のようにも書くことができる。一方で、閉区画のように`right`を返すことはできない。

```Go
// 閉区画（境界の右側を返す）
// nums := []int{0, 0, 1, 1, 1, 1, 1, 1, 1, 1}; target := 1;のとき、インデックス2が返される
func search(nums []int, target int) int {
  left, right := 0, len(nums)-1
  for left <= right {
    mid := left + (right-left)/2
    if nums[mid] < target {
      left = mid + 1
    } else {
      right = mid - 1
    }
  }
  if left >= len(nums) || nums[left] != target {
    return -1
  }
  return left
}
```
