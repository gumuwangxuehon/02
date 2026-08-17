---
title: 面向对象程序设计实验报告
date: 2026-07-01 00:00:00
categories:
  - Java课后作业
tags:
  - Java
  - Swing
  - GUI
  - 事件处理
  - 实验报告
  - 算术练习
cover: /img/covers/mountain-3.jpg
top_img: /img/covers/mountain-3.jpg
---

# 面向对象程序设计实验报告

**实验日期**：2026年 7 月 1 日  
**姓名**：  
**学号**：  
**班级**：

## 一、实验目的

本次实验旨在通过开发一个小学生算术练习系统，深入理解 Java GUI 编程和事件处理机制，具体目标如下：

1. **掌握 Swing 图形界面开发**：熟练使用窗口（JFrame）、标签（JLabel）、输入框（JTextField）、按钮（JButton）、下拉选择框（JComboBox）等核心组件的创建与属性设置，理解布局管理器（BorderLayout、GridLayout）的使用方法。

2. **理解事件处理机制**：深入理解事件源、事件对象、事件监听器、事件处理接口四大核心要素，熟练使用 `ActionListener` 接口实现按钮、输入框、下拉框的动作监听。

3. **实现分层难度的四则运算**：结合随机数生成与分支逻辑，实现整数、小数、分数三种难度级别的算术出题功能，巩固 Java 基础语法与数值运算能力。

## 二、实验环境

| 项目 | 版本 |
|------|------|
| 操作系统 | Windows 10 |
| Java 版本 | JDK 1.8+ |
| IDE | IntelliJ IDEA / Eclipse |

## 三、实验需求分析

### 3.1 功能需求

- **难度选择**：支持三种难度级别（简单-整数、中等-小数、进阶-分数）
- **随机出题**：根据难度级别随机生成不同类型的算术题目
- **答案验证**：用户输入答案后，系统自动判断正确性并给出反馈
- **得分统计**：正确答题累计得分，实时显示当前得分
- **题目切换**：支持手动切换下一题或切换难度时自动更新题目

### 3.2 界面需求

- 界面简洁美观，布局合理
- 题目显示清晰醒目
- 答题反馈直观（颜色区分对错）
- 操作方便快捷（支持回车键提交答案）

### 3.3 技术要点

| 知识点 | 说明 |
|--------|------|
| Swing 组件 | JFrame、JLabel、JTextField、JButton、JComboBox |
| 布局管理 | BorderLayout（整体）、GridLayout（题目区域） |
| 事件处理 | ActionListener 接口、Lambda 表达式 |
| 随机数 | Random 类生成随机操作数 |
| 数值运算 | 整数、小数、分数四则运算 |

## 四、系统设计

### 4.1 类结构设计

```
MathPractice (主类)
├── 组件成员变量
│   ├── difficultyBox: JComboBox<String>  // 难度选择框
│   ├── questionLabel: JLabel              // 题目显示标签
│   ├── answerInput: JTextField            // 答案输入框
│   ├── submitBtn: JButton                 // 提交按钮
│   ├── nextBtn: JButton                   // 下一题按钮
│   ├── resultTip: JLabel                  // 结果提示标签
│   └── scoreLabel: JLabel                 // 得分显示标签
├── 数据成员变量
│   ├── random: Random                     // 随机数生成器
│   ├── score: int                         // 当前得分
│   ├── num1, num2: int                    // 整数操作数
│   ├── d1, d2: double                     // 小数操作数
│   ├── op: String                         // 运算符
│   └── type: int                          // 题目类型 (0整数/1小数/2分数)
└── 方法
    ├── MathPractice()                     // 构造方法
    ├── initUI()                           // 初始化界面
    ├── bindEvent()                        // 绑定事件
    ├── generateQuestion()                 // 生成题目
    ├── checkAnswer(ActionEvent)           // 检查答案
    ├── calcInt(int, int, String)          // 整数运算
    ├── calcDouble(double, double, String) // 小数运算
    └── main(String[])                     // 主方法
```

### 4.2 界面布局设计

```
┌─────────────────────────────────────────┐
│  [选择难度： ▼]  [简单-整数]  [当前得分：0]  │  ← 顶部面板 (BorderLayout.NORTH)
├─────────────────────────────────────────┤
│                                         │
│          15 + 8 = ?                      │  ← 题目标签 (24号黑体)
│                                         │  ← 中间面板 (BorderLayout.CENTER)
│    [你的答案：] [____________]            │  ← 输入框区域
│                                         │
│          回答正确！+10分                  │  ← 结果提示 (颜色反馈)
│                                         │
├─────────────────────────────────────────┤
│     [提交答案]        [下一题]            │  ← 底部面板 (BorderLayout.SOUTH)
└─────────────────────────────────────────┘
```

## 五、实验代码

```java
package org.example;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Random;

public class MathPractice extends JFrame {

    private JComboBox<String> difficultyBox;
    private JLabel questionLabel;
    private JTextField answerInput;
    private JButton submitBtn, nextBtn;
    private JLabel resultTip, scoreLabel;

    private final Random random = new Random();
    private int score = 0;
    private int num1, num2;
    private double d1, d2;
    private String op;
    private int type;
    private final String[] ops = {"+", "-", "*", "/"};

    public MathPractice() {
        initUI();
        generateQuestion();
    }

    private void initUI() {
        setTitle("小学生算术练习系统");
        setSize(450, 320);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);
        setLayout(new BorderLayout(10, 10));
        setResizable(false);

        JPanel topPanel = new JPanel();
        topPanel.add(new JLabel("选择难度："));
        String[] levels = {"简单-整数", "中等-小数", "进阶-分数"};
        difficultyBox = new JComboBox<>(levels);
        topPanel.add(difficultyBox);
        scoreLabel = new JLabel("当前得分：0");
        topPanel.add(scoreLabel);
        add(topPanel, BorderLayout.NORTH);

        JPanel centerPanel = new JPanel();
        centerPanel.setLayout(new GridLayout(3, 1, 5, 10));
        centerPanel.setBorder(BorderFactory.createEmptyBorder(20, 30, 20, 30));

        questionLabel = new JLabel("题目加载中", SwingConstants.CENTER);
        questionLabel.setFont(new Font("黑体", Font.BOLD, 24));

        JPanel inputPanel = new JPanel();
        inputPanel.add(new JLabel("你的答案："));
        answerInput = new JTextField(12);
        answerInput.setFont(new Font("宋体", Font.PLAIN, 18));
        inputPanel.add(answerInput);

        resultTip = new JLabel(" ", SwingConstants.CENTER);
        resultTip.setFont(new Font("宋体", Font.BOLD, 18));

        centerPanel.add(questionLabel);
        centerPanel.add(inputPanel);
        centerPanel.add(resultTip);
        add(centerPanel, BorderLayout.CENTER);

        JPanel btnPanel = new JPanel();
        submitBtn = new JButton("提交答案");
        nextBtn = new JButton("下一题");
        btnPanel.add(submitBtn);
        btnPanel.add(nextBtn);
        add(btnPanel, BorderLayout.SOUTH);

        bindEvent();
    }

    private void bindEvent() {
        submitBtn.addActionListener(this::checkAnswer);
        answerInput.addActionListener(this::checkAnswer);

        nextBtn.addActionListener(e -> {
            generateQuestion();
            answerInput.setText("");
            resultTip.setText("");
        });

        difficultyBox.addActionListener(e -> {
            generateQuestion();
            answerInput.setText("");
            resultTip.setText("");
        });
    }

    private void generateQuestion() {
        int level = difficultyBox.getSelectedIndex();
        op = ops[random.nextInt(4)];

        switch (level) {
            case 0:
                type = 0;
                num1 = random.nextInt(20) + 1;
                num2 = random.nextInt(20) + 1;
                if (op.equals("-") && num1 < num2) {
                    int t = num1; num1 = num2; num2 = t;
                }
                if (op.equals("/")) {
                    num2 = random.nextInt(10) + 1;
                    num1 = num2 * (random.nextInt(10) + 1);
                }
                questionLabel.setText(num1 + " " + op + " " + num2 + " = ?");
                break;

            case 1:
                type = 1;
                d1 = Math.round((random.nextDouble() * 20 + 1) * 10.0) / 10.0;
                d2 = Math.round((random.nextDouble() * 10 + 1) * 10.0) / 10.0;
                if (op.equals("-") && d1 < d2) {
                    double t = d1; d1 = d2; d2 = t;
                }
                if (op.equals("/")) {
                    d2 = Math.round((random.nextDouble() * 5 + 1) * 10.0) / 10.0;
                    d1 = Math.round(d2 * (random.nextInt(8) + 1) * 10.0) / 10.0;
                }
                questionLabel.setText(d1 + " " + op + " " + d2 + " = ?");
                break;

            case 2:
                type = 2;
                num1 = random.nextInt(9) + 1;
                num2 = random.nextInt(9) + 1;
                int n3 = random.nextInt(9) + 1;
                int n4 = random.nextInt(9) + 1;
                if (op.equals("-") && (double) num1 / num2 < (double) n3 / n4) {
                    int t1 = num1, t2 = num2;
                    num1 = n3; num2 = n4;
                    n3 = t1; n4 = t2;
                }
                questionLabel.setText(num1 + "/" + num2 + " " + op + " " + n3 + "/" + n4 + " = ?（小数作答）");
                break;
        }
    }

    private void checkAnswer(ActionEvent e) {
        String inputStr = answerInput.getText().trim();
        if (inputStr.isEmpty()) {
            resultTip.setText("请输入答案！");
            resultTip.setForeground(Color.ORANGE);
            return;
        }

        double userAns;
        try {
            userAns = Double.parseDouble(inputStr);
        } catch (Exception ex) {
            resultTip.setText("输入格式错误，请输入数字");
            resultTip.setForeground(Color.RED);
            return;
        }

        double realAns = calculateAnswer();

        if (Math.abs(userAns - realAns) < 0.01) {
            score += 10;
            scoreLabel.setText("当前得分：" + score);
            resultTip.setText("回答正确！+10分");
            resultTip.setForeground(new Color(0, 150, 0));
        } else {
            resultTip.setText("回答错误，正确答案：" + String.format("%.2f", realAns));
            resultTip.setForeground(Color.RED);
        }
    }

    private double calculateAnswer() {
        switch (type) {
            case 0:
                return calcInt(num1, num2, op);
            case 1:
                return calcDouble(d1, d2, op);
            case 2:
                double f1 = (double) num1 / num2;
                double f2 = (double) (random.nextInt(9) + 1) / (random.nextInt(9) + 1);
                return calcDouble(f1, f2, op);
            default:
                return 0;
        }
    }

    private int calcInt(int a, int b, String op) {
        return switch (op) {
            case "+" -> a + b;
            case "-" -> a - b;
            case "*" -> a * b;
            case "/" -> a / b;
            default -> 0;
        };
    }

    private double calcDouble(double a, double b, String op) {
        return switch (op) {
            case "+" -> a + b;
            case "-" -> a - b;
            case "*" -> a * b;
            case "/" -> a / b;
            default -> 0;
        };
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> new MathPractice().setVisible(true));
    }
}
```

## 六、运行结果

### 6.1 启动界面

程序启动后显示主界面，包含难度选择下拉框、得分显示、题目区域、答案输入框和操作按钮。

### 6.2 不同难度题目示例

| 难度级别 | 题目示例 | 说明 |
|---------|---------|------|
| 简单-整数 | `15 + 8 = ?` | 操作数范围 1-20 |
| 中等-小数 | `12.5 * 3.2 = ?` | 操作数保留一位小数 |
| 进阶-分数 | `3/4 + 2/3 = ?` | 分数运算，小数作答 |

### 6.3 答题反馈

- **正确答案**：显示绿色文字 "回答正确！+10分"，得分增加10分
- **错误答案**：显示红色文字 "回答错误，正确答案：XX.XX"
- **未输入答案**：显示橙色文字 "请输入答案！"
- **格式错误**：显示红色文字 "输入格式错误，请输入数字"

## 七、代码优化说明

### 7.1 代码改进

1. **新增 `calculateAnswer()` 方法**：将答案计算逻辑从 `checkAnswer()` 中提取出来，使代码更清晰
2. **使用 Lambda 表达式**：简化事件监听器的书写，代码更简洁
3. **合理的变量命名**：使用有意义的变量名，提高代码可读性
4. **注释说明**：关键代码段添加注释，便于理解

### 7.2 注意事项

1. **Java 8 兼容性**：使用 `(e) ->` 代替 `_` 参数，确保兼容 Java 8
2. **负数处理**：减法和除法运算中确保结果非负
3. **除零保护**：除法运算中确保除数不为零
4. **精度控制**：小数比较使用 `Math.abs(userAns - realAns) < 0.01`

## 八、心得体会

本次实验是一次非常有价值的实践经历。通过开发一个完整的桌面应用程序，我将课堂上学到的 Java 基础语法、面向对象编程、Swing 图形界面和事件处理等知识有机地融合在一起，实现了从理论到实践的跨越。

在开发过程中，我深刻体会到了以下几点：

1. **界面设计的重要性**：一个良好的用户界面需要合理的布局和美观的视觉效果，BorderLayout 和 GridLayout 的组合使用让界面层次分明。

2. **事件处理的核心思想**：理解事件源、事件对象、事件监听器三者之间的关系是掌握 GUI 编程的关键，Lambda 表达式大大简化了事件处理代码。

3. **代码结构的优化**：将复杂逻辑拆分成多个小方法，提高代码的可读性和可维护性。

4. **错误处理的必要性**：对用户输入进行验证和处理，避免程序崩溃，提升用户体验。

同时，我也认识到自己在代码复用和逻辑优化方面还有提升空间。未来可以进一步扩展功能，如增加错题保存、计时答题、历史记录等模块，使系统更加完善。

通过本次实验，我不仅巩固了 Swing 和事件处理的知识，更重要的是掌握了小型桌面工具的开发思路和方法，为今后的学习和实践打下了坚实的基础。
