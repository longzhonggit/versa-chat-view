# Canvas控制功能更新

## 新增功能

为每个canvas块的右上角添加了两个控制按钮：

### 1. 展开/收缩按钮 📖
- **图标**：向上/向下箭头
- **功能**：点击可展开或收缩canvas内容区域
- **状态**：
  - 展开状态：显示向上箭头，点击后收缩内容
  - 收缩状态：显示向下箭头，点击后展开内容

### 2. 删除按钮 ❌
- **图标**：X符号
- **功能**：点击删除整个canvas块
- **交互**：鼠标悬停时变为红色
- **确认**：直接删除，无确认弹窗

## 实现细节

### 数据结构更新
```typescript
export interface CanvasData {
  // ... 现有属性
  collapsed?: boolean  // 新增：收缩状态
  id: string          // 新增：唯一标识符
}
```

### 服务层功能
```typescript
// CanvasService新增方法
removeCanvas(id: string)     // 删除指定canvas项
toggleCanvas(id: string)     // 切换展开/收缩状态
```

### 组件更新
1. **canvas-viewer.tsx** - 为HTML、Markdown、ImageShow类型添加控制按钮
2. **file-preview-viewer.tsx** - 为文件预览类型添加控制按钮

### 视觉效果
- **按钮样式**：灰色图标，悬停时高亮
- **删除按钮**：悬停时变红色
- **展开/收缩**：内容区域的显示/隐藏动画
- **ID生成**：使用时间戳+随机字符串确保唯一性

## 使用方法

### 测试步骤
1. 访问 http://localhost:3000
2. 点击任意"测试"按钮创建canvas内容
3. 观察每个canvas块右上角的控制按钮
4. 点击展开/收缩按钮测试状态切换
5. 点击删除按钮测试删除功能

### 支持的Canvas类型
- ✅ **HTML Canvas** - 完全支持
- ✅ **Markdown Canvas** - 完全支持  
- ✅ **Image Show** - 完全支持
- ✅ **文件预览** - 完全支持（PDF、Excel、Word、Text、Markdown、HTML）

### 状态管理
- **实时同步**：所有监听器立即接收状态变化
- **数据一致性**：服务层统一管理状态
- **组件响应**：UI自动响应数据变化

## 代码架构

### 事件流
1. **用户点击** → 组件处理函数
2. **调用服务** → CanvasService方法
3. **状态更新** → 数据模型变化
4. **通知监听器** → 所有组件收到更新
5. **UI重渲染** → 界面立即反映变化

### 删除机制
- 使用特殊的 `remove-signal` 类型
- 监听器根据信号类型过滤项目
- 避免复杂的事件系统设计

### 扩展性
- 新的canvas类型只需实现控制按钮渲染
- 状态管理逻辑在服务层统一处理
- 组件间解耦，易于维护

---

**总结**：成功为所有canvas块添加了展开/收缩和删除功能，提升了用户体验和界面管理能力。系统保持了良好的架构设计和扩展性。 