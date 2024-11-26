## *S - Single Responsibility Principle (SRP):*

* A class should have only one reason to change.

* In Angular, any service, component, or a directive should be able to have just a single responsibility to be fully maintainable.

'''import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class StudentService {
  private students: string[] = [];

  addStudent(name: string) {
    this.students.push(name);
  }

  deleteStudent(index: number) {
    this.students.splice(index, 1);
  }

  getStudents() {
    return [...this.students];
  }
}

## *Open/Closed Principle (OCP)*

* Software entities should be open for extension but closed for modification.

* The applyDiscount method uses strategies that can be extended without changing existing code.

'''import { Injectable } from '@angular/core';

interface DiscountStrategy {
  calculate(price: number): number;
}

class NoDiscount implements DiscountStrategy {
  calculate(price: number): number {
    return price;
  }
}

class SeasonalDiscount implements DiscountStrategy {
  calculate(price: number): number {
    return price * 0.9;
  }
}

@Injectable({
  providedIn: 'root',
})
export class InventoryService {
  applyDiscount(price: number, strategy: DiscountStrategy): number {
    return strategy.calculate(price);
  }
}

## *Liskov Substitution Principle (LSP)*

* Subtypes must be substitutable for their base types without altering the program's correctness.

* Both EmailNotification and SMSNotification can replace the Notification base class without issues.

'''abstract class Notification {
  abstract send(message: string): void;
}

class EmailNotification extends Notification {
  send(message: string): void {
    console.log(`Email sent: ${message}`);
  }
}

class SMSNotification extends Notification {
  send(message: string): void {
    console.log(`SMS sent: ${message}`);
  }
}

function notifyUser(notification: Notification, message: string) {
  notification.send(message);
}

const emailNotifier = new EmailNotification();
notifyUser(emailNotifier, 'Welcome to our platform!');

## *Interface Segregation Principle (ISP)*

* Clients should not be forced to depend on methods they do not use.

* The interfaces are small and focused, ensuring classes implement only what they need.

'''interface BasicReport {
  generate(): string;
}

interface DetailedReport extends BasicReport {
  generatePDF(): void;
}

class SummaryReport implements BasicReport {
  generate(): string {
    return 'Summary Report';
  }
}

class FullReport implements DetailedReport {
  generate(): string {
    return 'Detailed Report';
  }

  generatePDF(): void {
    console.log('PDF Generated');
  }
}

## *Dependency Inversion Principle (DIP)*

* High-level modules should not depend on low-level modules. Both should depend on abstractions.

* The LoggerComponent depends on the Logger abstraction, making it easy to swap implementations.

'''export interface Logger {
  log(message: string): void;
}

export class ConsoleLogger implements Logger {
  log(message: string): void {
    console.log(message);
  }
}

import { Component, Inject } from '@angular/core';
import { Logger } from '../../services/logger.service';

@Component({
  selector: 'app-logger',
  template: '<button (click)="logMessage()">Log Message</button>',
})
export class LoggerComponent {
  constructor(@Inject('Logger') private logger: Logger) {}

  logMessage() {
    this.logger.log('This is a log message');
  }
}

## *Real-World Use Cases*

* SRP: Separate form validation into a service for cleaner components.

* OCP: Add new payment methods in an e-commerce app by implementing new strategies.

* LSP: Use different notification types (e.g., email, SMS) in the same flow.

* ISP: Design small, focused interfaces for specific functionalities in services.

* DIP: Inject services in Angular components via dependency injection.
