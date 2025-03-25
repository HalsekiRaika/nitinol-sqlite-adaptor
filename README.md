# nitinol-sqlite-adaptor

This is a simple adaptor to use Nitinol with SQLite databases.

## Usage
```rust
#[tokio::test]
async fn main() -> anyhow::Result<()> {
    // Initialize the sqlite inmemory eventstore
    let eventstore = SqliteEventStore::setup("sqlite://:memory:").await?;
    
    // Initialize the event writer
    let writer = EventWriter::new(eventstore.clone()).set_retry(5);
    
    // Install writer into global
    nitinol::setup::set_writer(writer);
    
    // It is recommended that the actual saving method 
    // be done at the beginning of the EventApplicator process.
    // 
    // #[async_trait]
    // impl EventApplicator<EntityEvent> for Entity {
    //     #[tracing::instrument(skip_all)]
    //     async fn apply(&mut self, event: EntityEvent, ctx: &mut Context) {
    //         self.persist(&event, ctx).await;
    //         
    //         match event {
    //             EntityEvent::Created => {
    //                 tracing::debug!("Entity created.");
    //             }
    //         }
    //     }
    // }
}
```