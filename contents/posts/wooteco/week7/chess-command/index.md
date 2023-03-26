---
title: "체스 게임 - 커맨드 구현 일지"
date: 2023-03-26
tags:
  - 우테코
  - wooteco
  - 우아한테크코스
series: "wooteco"
---

## 체스 게임

이번주는 체스 게임 미션을 하는데 보냈다. 체스 게임에는 세 가지의 명령어가 존재한다.

```
> 게임 시작 : start
> 게임 종료 : end
> 게임 이동 : move source위치 target위치 - 예. move b2 b3
```

<br/>

명령어는 콘솔에서 `List<String>`으로 입력받는다. 

```
start의 경우 : List.of("string")
move의 경우 : List.of("move", "b2", "b3")
```



입력받은 명령어에 대응하는 동작을 실행시키기 위해선 어떻게 해야할까?<br/>



### 📌 if문을 사용해 커맨드에 따른 로직 실행

가장 먼저 떠오르는 방법은  `if문`을 통해 입력받은 명령어가 어떤 명령어인지를 판단하고, 그에 맞는 로직을 실행시키는 방법이었다.

```java
// ChessGameController
private ChessGame executeCommand(ChessGame chessGame, List<String> inputCommand) {
        Command command = inputCommand.get(COMMAND_HEAD_INDEX);
    
        if (command == Command.START) {
            // start 로직
        }
    
        if (command == Command.MOVE) {
            // move 로직
        }
    
        if (command == Command.END) {
            // end 로직
        }
    
        return chessGame;
}
```

<br/>

그러나 `if문`을 사용하면, `Command`의 종류가 늘어나는 만큼, `if문`의 개수도 증가하게 된다. `Command`를 일일이 조건문으로 확인하지 않고 로직을 실행할 수 있는 방법은 없을까?



### 📌 Enum객체에 함수형 인터페이스 사용 

`Command`는 `Enum`타입이다. `Enum`의 각 상수는 자신만의 변수를 가질 수 있다. 아래 String타입의 command가 그 예시이다. 

```java
public enum Command {
    START("start"),
    MOVE("move"),
    END("end");

    private final String command;

    Command(String command) {
        this.command = command;
    }
}
```

그렇다면 `Command`의 각 로직도 함수형 인터페이스를 사용해 변수로 가지게 하면 어떨까?

<br/>

```java
// 커맨드의 로직을 실행시키는 CommandAction 인터페이스를 생성했다!
interface CommandAction {
        ChessGame execute(final ChessGame chessGame, List<String> commands);
}

// 커맨드가 Start일 때 실행되는 로직 StartAction을 생성했다!
public class StartAction implements CommandAction {
    @Override
    ChessGame execute(final ChessGame chessGame, List<String> commands){
        // validateCommands(commands);
        return chessGame.start();
    }
}

// 커맨드가 Move일 때 실행되는 로직 MoveAction을 생성했다!
public class MoveAction implements CommandAction {
    @Override
    ChessGame execute(final ChessGame chessGame, List<String> commands){
        // validateCommands(commands);
        String currentPosition = commands.get(1);
        String nextPosition = commands.get(2);
        return chessGame.move(currentPosition, nextPosition);
    }
}

// 커맨드가 End일 때 실행되는 로직 EndAction을 생성했다!
public class EndAction implements CommandAction {
    @Override
    ChessGame execute(final ChessGame chessGame, List<String> commands){
        // validateCommands(commands);
        return chessGame.end();
    }
}
```

<br/>

`CommandAction` 인터페이스를 사용해 `Enum`객체에서 `execute()` 메소드를 호출하면, 바로 대응하는 로직을 실행시킬 수 있게 변경했다.

```java
public enum Command {
    START("start", new StartAction()),
    MOVE("move", new MoveAction()),
    END("end", new EndAction());

    private final String command;
    private final CommandAction commandAction;
    
    public ChessGame execute(ChessGame chessGame, List<String> commands){
        return this.commandAction.execute(chessGame, commands);
    }
}
```

<br/>

* 기존 `Command`실행 로직

```java
// ChessGameController
private ChessGame executeCommand(ChessGame chessGame, List<String> inputCommand) {
        Command command = inputCommand.get(COMMAND_HEAD_INDEX);
    
        if (command == Command.START) {
            // start 로직
        }
    
        if (command == Command.MOVE) {
            // move 로직
        }
    
        if (command == Command.END) {
            // end 로직
        }
    
        return chessGame;
}
```

<br/>

* 바뀐 `Command` 실행 로직

```java
// ChessGameController
private ChessGame executeCommand(ChessGame chessGame, List<String> inputCommand) {
    Command command = inputCommand.get(COMMAND_HEAD_INDEX);
    command.execute(chessGame, inputCommand);
}
```

<br/>

#### Command를 저장해야 하는 경우는?

수행한 `Command`를 전부 저장해야한다면 어떻게 될까?<br/>

* 실행한 명령어는 취소가 가능하다.
* 명령어 실행 히스토리를 저장한다.

위와 같은 요구사항이 추가된다면, `Command` 내용을 전부 저장해야한다. `Action`에서 커맨드 내용을 저장하면 어떨까? 

```java
public class MoveAction implements Action{
    private List<String> commands; // 커맨드 내용 저장
    
    @Override
    public void execute(ChessGame chessGame, List<String> commands) {
        // validateCommandsLength(commands);
        this.commands = commands;
        String currentPosition = commands.get(1);
        String nextPosition = commands.get(2);
        return chessGame.move(currentPosition, nextPosition);
    }
}
```

<br/>

커맨드 내용이 잘 저장되었는지 출력해보자.

```java
public class Application {
    public static void main(String[] args) {
        Command command1 = Command.MOVE;
        command1.execute(chessGame, Arrays.asList("move", "a2", "a3"));

        Command command2 = Command.MOVE;
        command2.execute(chessGame, Arrays.asList("move", "b7", "b5"));

        command1.printCommands();
        command2.printCommands();
    }
}
```

```
// 실행 결과
move b7 b5
move b7 b5
```

`move a2 a3`을 수행한 `command1`의 `Commands`가 `move b7 b5`로 저장되는 것을 볼 수 있다.<br/>

`Enum`은 상수의 집합으로, JVM이 자동으로  `public static final` 필드로 만든다. 각각의 상수들은 클래스 변수처럼 사용되며 같은 주소값을 참조한다.<br/>

때문에 각각의 `MOVE`커맨드를 저장할 수 없다.





### 📌 커맨드 패턴

각각의 커맨드를 저장하려면 어떻게 해야할까? `Command`객체를 `Enum `이 아닌  `Class`객체로 만들면 될 것 같다!<br/>

커맨드 패턴을 사용해서 구현해보았다.



#### Command 인터페이스 & ConcreteCommand 클래스 생성

```java
// Command 인터페이스 생성
public interface Command {
    ChessGame execute(ChessGame chessGame, List<String> commands);
}

// Command를 구현한 명령어 로직 클래스 생성
public class MoveCommand implements Command {
    private final List<String> commands;

    public MoveCommand(List<String> commands) {
        //validateCommandsLength(commands);
        this.commands = commands;
    }
    
    @Override
    public ChessGame execute(ChessGame chessGame, List<String> commands) {
        // move 로직
    }
}

```

<br/>

#### 명령어를 입력받아 적절한 Command객체를 돌려줄 로직이 필요

이제 `List<String> commands`를 입력받아 적절한 `Command`객체로 돌려줄 로직이 필요하다.<br/>

이걸 어떻게 예쁘게 구현할지 고민을 많이 했는데... `CommandType`이라는 `Enum`객체를 한 번 거쳐 `Command`를 반환해주게 했다. 더 좋은 방법이 있을 것 같은데, 나중에 찾으면 바꿀 예정이다.

```java
// ChessGameController
private void executeCommand(ChessGame chessGame, List<String> commands) {
        CommandType commandType = CommandType.getCommandType(commands);
        Command command = commandType.getCommand(commands);
        command.execute(chessGame, command);
}
```

<br/>

```java
public enum CommandType {
    START("start", StartCommand::new),
    MOVE("move", MoveCommand::new),
    END("end", EndCommand::new);
    
    private final String command;
    private final Function<List<String>, Command> commandGenerator;
    
    public static CommandType getCommandType(List<String> commands) {
    	// commands를 받아 적절한 CommandType을 반환
    }
    
    public Command getCommand(List<String> commands) {
        return commandGenerator.apply(commands);
    }
}
```



